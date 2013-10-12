---
layout: post
title: "Intelligently Deduplicating Records in Rails"
date: 2013-10-12 10:01
comments: true
categories: [Ruby on Rails]
---

I recently needed to write a deduplicate method in a [Rails app I built](http://www.preamp.fm). In the application, each artist `has_one` song. However, over the course of several months, several songs had been added to the database with the same `artist_id` as an existing song.

Based on [this helpful post](http://stackoverflow.com/questions/14124212/remove-duplicate-records-based-on-multiple-columns), my deduplicate method looked like this:

``` ruby Deduplicating Records with the Same Parent ID
class Song
  def self.deduplicate
    grouped = all.group_by { |song| song.artist_id }
    grouped.values.each do |artist_songs|
      first_one = artist_songs.shift
      artist_songs.each { |dup| dup.destroy }
    end
  end
```

This method groups every song by the song's artist. For groupings with more than one song (i.e. songs that have the same `artist_id`), it keeps the first song and deletes all the rest.

This works great, but it naively decides which duplicate record to destroy and which to keep.

To make the deduplicating method "smarter" we can use `sort` and `sort_by` to influence which duplicate will be kept. For example, if we wanted to keep the duplicate song with the highest play count, we could add this line before the `shift` method call:

```ruby
artist_songs.sort! {|a,b| b.play_count <=> a.play_count }
```

This would ensure that the song "kept" using the `shift` method is the song with the highest play count.

It's also possible to sort records by boolean values using `sort_by`. This was useful for my method, because in cases where an artist had two songs, I preferred to keep the song that had been reviewed by a trusted editor vs. one that had not.

The `sort` method doesn't work with booleans, so we instead use `sort_by` and replace the booleans with integers to achieve the sort. I used the following line of code before the `shift` call: 

```ruby
artist_songs.sort_by! { |song| song.reviewed ? 0 : 1 }
```

Now, I have a concise, reusable method I can use to intelligently de-duplicate my songs.

*Charles Worthington (<a href="http://www.twitter.com/cew821" title="Twitter">@cew821</a>) is a freelance product designer living in Washington, DC. He would love to <a href="mailto:%63%6f%6e%74%61%63%74@%67%72%61%79%64%75%63%6b%6c%61%62%73.%63%6f%6d" title="Email Us">hear from you</a>!*