---
layout: post
title: MVP + Dagger 2 on Android
date: '2016-01-22T08:07:00.001-08:00'
author: Ivan
tags:
- Android
categories:
- Android
modified_time: '2017-05-19T08:52:57.440-07:00'
blogger_id: tag:blogger.com,1999:blog-415098333644749081.post-538179353509770958
blogger_orig_url: http://invictuscode.blogspot.com/2016/01/mvp-dagger-2-in-android.html
---

MVP has become really popular in Android development. The reason is that it can help you to split logics from UI and even make your APP testable. For example, “Activity” and “Fragment” in Android sometimes cannot be written in a good way. If you don’t pay attention to activity and fragment, you might create many god objects in Android APP. Then your APP might have bad performance or some serious issues. In MVP, activity and fragment are tend to be views, they only update UI. They don’t deal with model and process data.  
Here’s an example of view:  

    public interface DetailView {    void onVideosLoaded(List<Video> videos);    void onReviewsLoaded(List<Review> reviews);    void onMovieAdded();    void onMovieDeleted();}

And the fragment which is responsible for updating UI implements the view.  

    public class DetailFragment extends Fragment implements DetailView {    ...    @Override    public void onVideosLoaded(List<Video> videos) {        // update videos on UI    }    @Override    public void onReviewsLoaded(List<Review> reviews) {        // update reviews on UI    }    @Override    public void onMovieAdded() {        // update UI    }    @Override    public void onMovieDeleted() {        // update UI    }    ...}

Ok, we finished our views, but we need the most important part, Presenter! Presenter is just like a middle man who communicates with mode and view. Let’s see how it’s done!  

    public class DetailPresenter {    private DetailView detailView;       private RestfulApi api;    ...    public void loadVideos(int id){        Subscription subscription = api.getVideos(id)                .subscribeOn(Schedulers.newThread())                .observeOn(AndroidSchedulers.mainThread())                .subscribe(videoResponse -> detailView.onVideosLoaded(videoResponse.getResults()));        subscriptions.add(subscription);    }    ...}

We create a method called “loadVideos” to load videos. When it finished loading, it will call view method to return videos. As you can see in the above code.  

    detailView.onVideosLoaded(videoResponse.getResults())

In this way, if you use detailPresenter.loadVideos in Fragment which implements DetailView, it will receive videos from onVideoLoaded callback.  

    public class DetailFragment extends Fragment implements DetailView {    ...private void loadVideosAndReviews(Movie movie){            presenter.loadVideos(movie.getId());            presenter.loadReviews(movie.getId());    }    ...}

You might wondering now, why is this MVP to do with Dagger 2? And what is Dagger2?  
Dagger2 is a dependency injection library for Android. It allows you to inject object into activity. So you don’t have to new classes or write bunch of constructors. If you are interested in Dagger2, you can check out [website](http://google.github.io/dagger/) or [this video](https://www.youtube.com/watch?v=SKFB8u0-VA0).  
You can use Dagger2 to provide presenter to a fragment.  

    @Modulepublic class DetailFragmentModule {    private Context context;    public DetailFragmentModule(Context context) {        this.context = context;    }    @Provides    DetailPresenter provideDetailPresenter(){        return new DetailPresenter(context);    }}

Then define Components to inject them.  

    @Component(modules = {DetailFragmentModule.class, ApiModule.class, DbModule.class})public interface DetailFragmentComponent {    void inject(DetailFragment fragment);}

Third, build them in Application with modules.  

    public class MovieApplication extends Application {    @Override    public void onCreate() {        super.onCreate();    }    public MainFragmentComponent getMainFragmentComponent(MainFragment mainFragment) {        return DaggerMainFragmentComponent.builder()                .apiModule(new ApiModule())                .dbModule(new DbModule(mainFragment.getContext()))                .mainFragmentModule(new MainFragmentModule(mainFragment.getContext())).build();    }    public DetailFragmentComponent getDetailFragmentComponent(DetailFragment detailFragment){        return DaggerDetailFragmentComponent.builder()                .apiModule(new ApiModule())                .dbModule(new DbModule(detailFragment.getContext()))                .detailFragmentModule(new DetailFragmentModule(detailFragment.getContext())).build();    }}

Finally, we get those injected objects in Fragment!  

    public class DetailFragment extends Fragment implements DetailView {    @Inject    DetailPresenter presenter;    @Nullable    @Override    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {        View rootView = inflater.inflate(R.layout.fragment_detail, container, false);        ButterKnife.bind(this, rootView);        setHasOptionsMenu(true);        ((MovieApplication) getActivity().getApplication()).getDetailFragmentComponent(this).inject(this);        return rootView;    }    ...

Using MVP with Dagger2 makes our APPs structured well and avoid huge Activity or Fragment classes, and what’s more? It can actually make your APP testable. Because you seperated data processing logics from view. Then it’s easier for us to write tests against presenters! If you are interested in Testing MVP, you can check out [google codelab](https://codelabs.developers.google.com/codelabs/android-testing/index.html?index=../../index#0). It contains materials which let you unstand MVP, unit testing, and UI testing on Android.  
At last, I would like to share my Udacity android nanodegree project. In this project, I use MVP and Dagger as well. You can check it out on my [github repository](https://github.com/ivanisidrowu/AND-project1n2-popmovies)!