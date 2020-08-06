---
layout: post
title:  "Customize Your Own Retrofit"
date:   2017-12-31 10:00:00
author: Ivan
categories: Android
tags: Android
---
Retrofit is a very useful when sending HTTP requests. Also, it's extremely simple to write. In this post, I will describe how to write a custom Retrofit client and how to extend it. If you haven't got familiar with Retrofit, I suggest that you can go to its [website][1] to know some basics about it.

(In fact, I wanted to write this post 3 months ago, but I need to prepare and practice algorithm. :D Maybe I will share some interesting tips for those who want to practice algorithm.)

## Why Retrofit?
* Easy to use
* Great extensibility
* No Android dependency
* Good testability
* It supports several types of calls
* It supports different types of converters

When you are using Retrofit, you don't need to write a bunch of code boilerplates to send HTTP requests. The only thing we have to write is to declare the APIs in an interface. This interface allows us to extend and maintain APIs efficiently. For example, if you want to add a HTTP GET request into an API, just simply add a method into the interface with params and @GET annotations. Additionally, this provides good testability to tests. When it comes to writing Android unit tests, Android dependency is definitely an essential factor we need to consider. Without Android dependency, unit testing code can be easier to write. We don't have to mock Android classes or use 3rd party libs to mock them. Retrofit also support different types of converters and calls. So we can customize Retrofit with these classes. I will explain how to use them in this post.

## Enhanced Funtions
For most of senarios, we customize Retrofit to enhance features which original Retrofit doesn't provide. You can see the list below. It contains some enhanced functions, and I will use these features as example to explain how to customize those features with Retrofit.

* Custom calls
* Custom call adapters
* Retry requests automatically
* Dealing with fire-and-forget cases in Retrofit
* Cancel requests by tags
* Managing requests

## Custom calls and adapters
In normal cases, we don't need to customize our own calls or call adapters to send requests. However, if you want to gain more controls of requests. It's for the best to override them. In order to enhance the functions I mentioned, we override calls and call adapters.

```java
public class RetrofitCall<T> implements Call<T> {
    private Call<T> call;
    private Executor executor;
    private String logTag = RetrofitCall.class.getSimpleName();

    public RetrofitCall(Call<T> call, Executor executor) {
        this.call = call;
        this.executor = executor;
    }

    @Override
    public Response<T> execute() throws IOException {
        return call.execute();
    }

    @Override
    public void enqueue(final Callback<T> callback) {
        call.enqueue(new Callback<T>() {
            @Override
            public void onResponse(final Call<T> call, final Response<T> response) {
                if (isCallSuccess(response)) {
                    handleSuccessResponse(call, response, callback);
                } else {
                    handleErrorResponse(call, new IOException("Call failed!"), callback);
                }
            }

            @Override
            public void onFailure(final Call<T> call, final Throwable t) {
                handleErrorResponse(call, t, callback);
            }
        });
    }

    @WorkerThread
    private void handleErrorResponse(final Call<T> call, final Throwable t, final Callback<T> callback) {
        executor.execute(new Runnable() {
            @Override
            public void run() {
                if (null == call || call.isCanceled()) {
                    return;
                }
                callback.onFailure(call, t);
                notifyCallFinished();
            }
        });
    }

    @WorkerThread
    private void handleSuccessResponse(final Call<T> call, final Response<T> response, final Callback<T> callback) {
        executor.execute(new Runnable() {
            @Override
            public void run() {
                if (null == call || call.isCanceled()) {
                    return;
                }
                callback.onResponse(call, response);
                notifyCallFinished();
            }
        });
    }

    @Override
    public boolean isExecuted() {
        return call.isExecuted();
    }

    @Override
    public void cancel() {
        call.cancel();
    }

    @Override
    public boolean isCanceled() {
        return call.isCanceled();
    }

    @Override
    public RetrofitCall<T> clone() {
        RetrofitCall<T> clone = new RetrofitCall<>(call.clone(), executor);
        clone.setRequestTag(requestTag);
        clone.setLogTag(logTag);
        return clone;
    }

    @Override
    public Request request() {
        return call.request();
    }

    private boolean isCallSuccess(Response<T> response) {
        int httpStatusCode = response.code();
        return (httpStatusCode >= HttpURLConnection.HTTP_OK && httpStatusCode < HttpURLConnection.HTTP_BAD_REQUEST);
    }
}
```
As you can see, I implement Call interface. It has two constructors, Call<T> and Executor. Call<T> is the original Retrofit Call object, we just wrap it into this new class. Executor is to send requests in specific thread. There is a pitfall that you have to modify code in order to send response back to UI thread. By default, Retrofit executes custom calls on background thread when you override your custom calls. So I throw the response in executor to let it send back to UI thread.

After we finished the custom call, we can start writing call adapter.

```java
public class RetrofitCallAdapter<T> implements CallAdapter<T, RetrofitCall<T>> {

    private final Type responseType;
    private final Executor executor;

    public RetrofitCallAdapter(Type responseType, Executor executor) {
        this.responseType = responseType;
        this.executor = executor;
    }

    @Override
    public Type responseType() {
        return responseType;
    }

    @Override
    public RetrofitCall<T> adapt(Call<T> call) {
        return new RetrofitCall<>(call, executor);
    }
}
```
Basically, it just make new custom calls for the call adapter factory to let Retrofit use them.

```java
public class RetrofitCallAdapterFactory extends CallAdapter.Factory {

    private static RetrofitCallAdapterFactory instance = null;
    private final Executor executor = new MainThreadExecutor();

    private RetrofitCallAdapterFactory() {

    }

    public static synchronized RetrofitCallAdapterFactory getInstance() {
        if (instance == null) {
            instance = new EtRetrofitCallAdapterFactory();
        }
        return instance;
    }

    @Override
    public CallAdapter<?, ?> get(Type returnType, Annotation[] annotations, Retrofit retrofit) {
        Class<?> rawType = getRawType(returnType);
        if (rawType == RetrofitCall.class && returnType instanceof ParameterizedType) {
            Type callReturnType = getParameterUpperBound(0, (ParameterizedType) returnType);
            return new RetrofitCallAdapter(callReturnType, executor);
        }
        return null;
    }
}
```
In call adapter factory, I create a main thread executor in order to send response back to UI thread.

## Retrying Requests
Retrofit does not retry calls itself, so we have to write by ourselves. In this example, I set retry limit to 2, and give it a retry count as 0. When it retrys 3 times, it stops retrying. There is a important thing we need to 

```java
private int retryLimit = 2;
private int retryCount = 0;

@Override
public void enqueue(final Callback<T> callback) {
    call.enqueue(new Callback<T>() {
        @Override
        public void onResponse(final Call<T> call, final Response<T> response) {
            if (isCallSuccess(response) || retryCount >= retryLimit) {
                handleSuccessResponse(call, response, callback);
            } else {
                handleErrorResponse(call, new IOException("Call failed!"), callback);
            }
        }

        @Override
        public void onFailure(final Call<T> call, final Throwable t) {
            if (isCanceled()) {
                // Do nothing.
            } else if (retryCount >= retryLimit) {
                handleErrorResponse(call, t, callback);
            } else {
                retryCount++;
                retry(callback);
            }
        }
    });
}

private void retry(final Callback<T> callback) {
    this.call = call.clone();
    enqueue(callback);
}
```
## Fire-and-Forget Cases
Sometimes, we send reqeusts, but we don't care what is the responses. Acutally, it quite simple. We just override Callback, and put it into enqueue method in Retrofit.

```java
public class EmptyRetrofitCallback<T> implements Callback<T> {

    @Override
    public void onResponse(Call<T> call, Response<T> response) {

    }

    @Override
    public void onFailure(Call<T> call, Throwable t) {

    }
}
```
## Managing Requests
By default, Retrofit don't have a class to let you manage requests, you have to manage yourself. It gave us a good extensibility to implement a class to provide features like cancelling all requests, cancelling requests by tags, and so on.

First, I implement a composite class, named CompositeCall, to manage requests. In this class, it has a list to save current running calls. When a call is executed or cancelled, it will be removed from the list automatically. I also create an interface to let this class listen to individual class. So it would know when to add or remove calls.

```java
public class CompositeCall implements RetrofitCall.CallListener {

    private static final String TAG = "CompositeCall";

    private List<RetrofitCall> list = new ArrayList<>();

    public void cancel(String requestTag) {
        Iterator<RetrofitCall> iterator = list.iterator();
        while (iterator.hasNext()) {
            RetrofitCall call = iterator.next();
            if (call.getRequestTag().equals(requestTag)) {
                call.cancel();
                Log.d(TAG, "cancel: The call with tag " + requestTag + " was cancelled.");
                iterator.remove();
            }
        }
    }

    public void cancelAll() {
        for (RetrofitCall call : list) {
            call.cancel();
        }

        list.clear();
    }
    
    public boolean isRunning(String requestTag) {
        if (TextUtils.isEmpty(requestTag)) {
            return false;
        }
        
        boolean isRunning = false;
        for (RetrofitCall call: list) {
            if (requestTag.equals(call.getRequestTag())) {
                isRunning = true;
                break;
            }
        }
        return isRunning;
    }

    @Override
    public void onFinished(RetrofitCall call) {
        call.detach();
        list.remove(call);
    }

    @Override
    public void onAdded(RetrofitCall call) {
        list.add(call);
    }
}
```

I add the code below in RetrofitCall to work with CompositeCall.

```java
private String requestTag = "";
private CallListener callListener;

public interface CallListener {
    void onFinished(RetrofitCall call);

    void onAdded(RetrofitCall call);
}

public RetrofitCall<T> setRequestTag(String requestTag) {
    this.requestTag = requestTag;
    return this;
}

public RetrofitCall<T> attachTo(CallListener listener) {
    callListener = listener;
    if (callListener == null) {
    } else {
        callListener.onAdded(this);
    }
    return this;
}

public void detach() {
    callListener = null;
}

private void notifyCallFinished() {
    if (callListener != null) {
        callListener.onFinished(this);
    }
}
```
Finally, we can use CompositeCall to manage requests like this...

```java
// Activity onCreate()
compositeCall = new CompositeCall();

// Send reqeust
api.getItem(appData.getApi())
                .setLogTag(tag)
                .setRequestTag(tag)
                .attachTo(compositeCalls)
                .enqueue(new Callback(){
                    // skip...
                });
                
// Activity onDestroy()
compositeCall.cancelAll();

// Or cancel by reqeust tag
compositeCall.cancel(tag);
```

It's very important to release or canel callbacks in the end of lifecycle! Don't forget to cancel requests in onDetroy() or onStop() to avoid memory leak!

## Unit Testing
I mentioned that Retrofit has good testability. If we override calls and adapters, how could we test them? Don't worry, it's not that hard to write unit tests against them! All of the calls are from Retrofit call, so we can write a fake call in unit test. Here is the test calls I write for RetrofitCall. I write this class by referencing [retrofit-mock][2].

```java
public class RetrofitTestCalls {

    public static <T> RetrofitCall<T> response(T successValue) {
        return new FakeCall<>(Response.success(successValue));
    }

    public static <T> RetrofitCall<T> response(Response<T> response) {
        return new FakeCall<>(response);
    }

    public static <T> RetrofitCall<T> failure(IOException failure) {
        return new FakeCall<>(failure);
    }

    static final class FakeCall<T> extends RetrofitCall<T> {
        private final Response<T> response;
        private final IOException error;
        private final AtomicBoolean canceled = new AtomicBoolean();
        private final AtomicBoolean executed = new AtomicBoolean();

        FakeCall(@NonNull Response<T> response) {
            super(null, null);

            this.response = response;
            this.error = null;
        }

        FakeCall(@NonNull IOException error) {
            super(null, null);
            this.response = null;
            this.error = error;
        }

        @Override
        public Response<T> execute() throws IOException {
            if (!executed.compareAndSet(false, true)) {
                throw new IllegalStateException("Already executed");
            }
            if (canceled.get()) {
                throw new IOException("canceled");
            }
            if (response != null) {
                return response;
            }
            throw error;
        }

        @SuppressWarnings("ConstantConditions") // Guarding public API nullability.
        @Override
        public void enqueue(Callback<T> callback) {
            if (callback == null) {
                throw new NullPointerException("callback == null");
            }
            if (!executed.compareAndSet(false, true)) {
                throw new IllegalStateException("Already executed");
            }
            if (canceled.get()) {
                callback.onFailure(null, new IOException("canceled"));
            } else if (response != null) {
                callback.onResponse(null, response);
            } else {
                callback.onFailure(null, error);
            }
        }

        @Override
        public boolean isExecuted() {
            return executed.get();
        }

        @Override
        public void cancel() {
            canceled.set(true);
        }

        @Override
        public boolean isCanceled() {
            return canceled.get();
        }

        @Override
        public RetrofitCall<T> clone() {
            if (response == null) {
                return new FakeCall<>(error);
            } else {
                return new FakeCall<T>(response);
            }
        }

        @Override
        public Request request() {
            if (response != null) {
                return response.raw().request();
            }
            return new Request.Builder().url("http://localhost").build();
        }
    }
}
```
Then, we can use them in tests. This mock class also gives us two advantages. First, we don't actually need to send request to real server and tests shall be much faster. Second, we can decide what to response in mock calls.

```java
@RunWith(PowerMockRunner.class)
public class MyPresenterImplTest {
    @Mock
    private IMyView mockView;
    @Mock
    private IRetrofitApi mockApi;

    private MyPresenterImpl presenter;

    @Before
    public void setUp() {
        presenter = new MyPresenterImpl(mockView, mockApi);
    }

    @Test
    public void getList() {
        final MyResponseModel response = TestModelUtil.getResponse();
        RetrofitCall<MyResponseModel> mockCall = getMockCall(response);
        presenter.getList();
        verify(mockView).setListRefreshing(true);
        assertTrue(mockCall.isExecuted());
        ArgumentCaptor<ArrayList> listArgumentCaptor = ArgumentCaptor.forClass(ArrayList.class);
        mockView.onListLoaded(listArgumentCaptor.capture());
    }

    private RetrofitCall<MyResponseModel> getMockCall(MyResponseModel resp) {
        RetrofitCall<MyResponseModel> mockCall = RetrofitTestCalls.response(Response.success(resp));
        when(mockApi.getList((String) anyObject())).thenReturn(mockCall);
        return mockCall;
    }
}
```
## Summary
Using Retrofit makes android development faster and better. It also makes code more concise and easier to read. Custom calls and composite calls can help us get rid off memory leaks if we use them properly. However, in my personal opinion, I recommend using [RxJava][3] with Retrofit and lamdba expression to avoid callback hell and gain more manipulations on program data flow.

[1]: http://square.github.io/retrofit/
[2]: https://github.com/square/retrofit/blob/master/retrofit-mock/src/main/java/retrofit2/mock/Calls.java
[3]: https://github.com/ReactiveX/RxJava