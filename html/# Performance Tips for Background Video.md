# Performance Tips for Background Video

> **Reference:** [Performance Tips for Background Video](https://calendar.perfplanet.com/2019/performance-tips-for-background-video/) &mdash; <cite>[Doug Sillars](https://dougsillars.com/)</cite>

I am posting these notes in order to get team members and clients to quickly refer to these performance tips regarding auto-playing video on their sites/apps.

Before we get into it, a word on **accessibility**:

* there **has** to be a way to pause or stop any HTML5 media element which plays automatically
  * [Techniques for WCAG 2.0: F93](https://www.w3.org/TR/WCAG20-TECHS/F93.html)
* videos, and motion in general, can be distracting or disorienting to many people
* if there is text in the video, some people will need more time to read the text so pausing it might be a good option for them

> Background videos are a great way to enhance the way your site delivers content – in addition to words and images, there is a motion in the background that can help your customers understand your services and why they should use them. But they can also go horribly wrong. The two biggest issues I have seen are:
>
> 1. Users scroll past the video before it starts. If they never see the video playing – the purpose of the video is lost.
> 1. BG video constantly starts and stops (stalls). Any time a video stalls, it initiates a context change, pulling the visitor out of the engagement you are trying to create.
>
> So, what can you do to ensure that the video added to your website will play quickly and efficiently? Here are a few tips that can help speed your video delivery.

## Alternate Formats

* webm formats are usually smaller, so include that format for browsers with webm support
  * results in faster delivery to the user

```html
<video autoplay muted>
    <source src= video.webm>
    <source src= video.mp4>
    Your Browser does not support video.
</video>
```

## Remove the audio track

* reduces file size

## Remove the poster

* downloads before the video plays (delays video)
* can also add flash of poster view before video starts

## Mobile downloads

* CSS `display: none` doesn't mean `download: none`
* use js to show video for specific screen sizes (< 768px)
  * otherwise the video still downloads even though the user will never see it!
* **OR** create a mobile specific, smaller file size version
  * make sure to test in portrait mode too

## Watch the length

* background videos are best suited for bite sized consumption
* for example: a 3 minute video would be way too long

## Lower the bit rate

* bit rate is measured in megabytes per second (MB/s or MBPS)
  * or even kilobytes per second (kB/s, or kBPS)
* to lower the bit rate...
  * decrease the dimensions/resolution of the video
  * increase the compression
    * this will affect quality so try what feels right
* many American cellular carriers throttle video playback to 1.5 MB/s
  * it is recommended to have video for users on cellular wifi that can play back at 1.3 MB/s to allow room for other files

## Test your video playback

* [StreamOrNot](https://dougsillars.github.io/StreamOrNot/)
  * free tool that measures the startup time and stalling characteristics of any video online
