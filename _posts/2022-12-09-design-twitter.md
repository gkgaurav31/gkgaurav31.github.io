---
layout: post
title: Design Twitter
date: 2022-12-09 23:30 +0530
author: "Gaurav Kumar"
tags: "java heap leetcode neetcode150" 
categories: "neetcode150"
---

## Problem Description

Design a simplified version of Twitter where users can post tweets, follow/unfollow another user, and is able to see the 10 most recent tweets in the user's news feed.

Implement the Twitter class:

- Twitter() Initializes your twitter object.
- void postTweet(int userId, int tweetId) Composes a new tweet with ID tweetId by the user userId. Each call to this function will be made with a unique tweetId.
- List<Integer> getNewsFeed(int userId) Retrieves the 10 most recent tweet IDs in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user themself. Tweets must be ordered from most recent to least recent.
- void follow(int followerId, int followeeId) The user with ID followerId started following the user with ID followeeId.
- void unfollow(int followerId, int followeeId) The user with ID followerId started unfollowing the user with ID followeeId.

[leetcode](https://leetcode.com/problems/design-twitter/description/)

### Solution

```java
class Twitter {

    //list of twitter users    
    Map<Integer, User> users;

    //mapping of userId with the User object
    public Twitter() {
        users = new HashMap<>();
    }
    
    public void postTweet(int userId, int tweetId) {

        //if existing user    
        if(users.containsKey(userId)){
            
            //get user object
            User user = users.get(userId);

            //create tweet object and increase the time for next tweet
            Tweet tweet = new Tweet(tweetId); Tweet.time++;

            //add the tweet to this user
            user.tweets.add(tweet);

        //if non-existing user
        }else{
            
            //create a new user with the input userId
            User user = new User(userId);

            //create tweeet object and increase the time for next tweet
            Tweet tweet = new Tweet(tweetId); Tweet.time++;

            //add the tweet to this user
            user.tweets.add(tweet);

            //add the user to list of users
            users.put(userId, user);
        }

    }
    
    public List<Integer> getNewsFeed(int userId) {

        //min heap based on timestamp
        //latest tweets will be at the top
        PriorityQueue<Tweet> pq = new PriorityQueue<>(); 

        //if the user is not already present, return empty
        if(!users.containsKey(userId)) return new ArrayList<>();

        //otherwise, get the user object
        User user = users.get(userId);
        //get the userId that this user is following
        Set<Integer> followings = user.following;

        //loop through followings list
        for(Integer followerId: followings){
            
            //get following user
            User follower = users.get(followerId);
            //get his tweets
            List<Tweet> tweets = follower.tweets;

            //add all tweets to min heap
            for(Tweet t: tweets){
                pq.add(t);
            }   

        }

        //answer list containing latest 10 tweets
        List<Integer> latestTweetId = new ArrayList<>();
        int s = 0;

        //get the latest 10 tweets from heap
        while(s<10 && !pq.isEmpty()){
            latestTweetId.add(pq.poll().tweetId);
            s++;
        }

        return latestTweetId;

    }
    
    public void follow(int followerId, int followeeId) {
        
        //handle the user where followerId or followeeId in not in the list of users
        if(!users.containsKey(followerId)){
            users.put(followerId, new User(followerId));
        }

        if(!users.containsKey(followeeId)){
            users.put(followeeId, new User(followeeId));
        }

        //get the user
        User user = users.get(followerId);

        //add the follower for that user
        user.following.add(followeeId);

    }
    
    public void unfollow(int followerId, int followeeId) {

        //get the user object
        User user = users.get(followerId);

        //remove followee for that user
        user.following.remove(followeeId);
    }
}

class User{

    int userId;
    List<Tweet> tweets;
    Set<Integer> following;

    //create new user
    User(Integer userId){
        this.userId = userId;
        tweets = new ArrayList<>();
        following = new HashSet<>();
        following.add(userId);//the user is its own follower
    }
}

class Tweet implements Comparable<Tweet>{

    int tweetId;
    int timestamp;

    public static int time = 0; //will use this to track timestamp for tweets

    Tweet(int tweetId){
        this.tweetId = tweetId;
        this.timestamp = Tweet.time;
    }

    public int compareTo(Tweet t2){
        if(this.timestamp == t2.timestamp) return 0;
        //if tweet1.timestamp > tweet2.timestamp
        //=> tweet1 is latest when compared to tweet2
        //=> we want tweet1 before tweet2 in ascending order so that we can add it to min heap
        if(this.timestamp > t2.timestamp) return -1; 
        else return 1;
    }

}

/**
 * Your Twitter object will be instantiated and called as such:
 * Twitter obj = new Twitter();
 * obj.postTweet(userId,tweetId);
 * List<Integer> param_2 = obj.getNewsFeed(userId);
 * obj.follow(followerId,followeeId);
 * obj.unfollow(followerId,followeeId);
 */
 ```
