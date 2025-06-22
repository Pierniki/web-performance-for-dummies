# Web Performance for Dummies: LCP edition

Let's talk about making users fall in love with your website. If you want to build something truly great, it needs to be more than just beautiful—it has to be fast. In this context, "performance" refers to how quickly your page loads, becomes interactive, and responds to user input. A slow site feels broken, but a fast, responsive site feels professional and reliable. It's the first and most important step to winning a user's trust and loyalty.

## Why Should We Care?

### 1. Performance Metrics Directly Influence Search Rankings

While there are dozens of performance metrics, it's crucial to understand that only a specific subset—the Core Web Vitals (LCP, INP, and CLS)—directly influences Google's search rankings. Google uses Field Data for this, which is collected from real users via the Chrome User Experience Report (CrUX). This is different from Lab Data, which you get from tools like Lighthouse and is essential for debugging before you ship.
The two are connected, however. Other technical metrics create a chain reaction that will impact your field data. For example, a slow Time to First Byte (TTFB) from your server, which you might diagnose in a lab test, delays the browser from even starting to render the page. This, in turn, delays the First Contentful Paint (FCP) and ultimately results in a poor LCP for your real users. So, while metrics like TTFB won't directly impact your ranking, they have a clear indirect effect. They can help you pinpoint the root cause of a poor Core Web Vitals score, allowing you to fix the underlying problem.

### 2. Higher User Satisfaction

Think about it from a user's perspective. Given two identical sites, users will always prefer the one that is faster. A snappy interface feels more professional and reliable. This has a massive impact on user retention when you consider it at scale.

> **For example:** If a slow load time causes just one extra visitor out of every hundred to leave, it might not seem significant. But for a site with 1 000 000 monthly visitors, that's 10 000 lost potential customers every single month. That adds up to 120 000 lost opportunities a year, which can translate into a significant amount of lost revenue.

### 3. Better Scalability and Cost Reduction

Efficient websites are simply cheaper to run. In a modern serverless architecture, performance optimization directly impacts your hosting bill.

## Today’s Core Web Vitals Highlight: LCP

This brings me to my highlight metric: Largest Contentful Paint. The official web.dev documentation defines it as:

> "The Largest Contentful Paint (LCP) metric reports the render time of the largest image or text block visible within the viewport, relative to when the page first started loading."

In even simpler terms, this metric measures the time from when the page first starts loading until the largest single element — typically an image or a block of text—becomes visible on the user's screen. It's a key measure of perceived loading speed.

## Common LCP Destroyers

So, how do we stop hurting our LCP scores? I see a few common culprits again and again.

### 1. Unoptimized Hero Images/Videos

This is the most frequent LCP killer, and it comes in several forms. Some of those are:
*   **Massive File Sizes and Outdated Formats:** Serving a 2 MB JPEG when a 300 KB AVIF or WebP image would suffice dramatically increases load time. Using modern image formats is essential.
*   **Serving the Wrong Size:** Your desktop hero image might be 2000px wide, but that same image should not be sent to a mobile device with a 400px screen. Using responsive images with the `srcset` attribute lets the browser download the most appropriate size, saving bandwidth.
*   **The `loading="lazy"` Trap:** Lazily loading an image tells the browser to deprioritize it. While this is great for below-the-fold content, applying `loading="lazy"` to your main hero image is a critical mistake, as it explicitly tells the browser to delay loading the most important visual element on your page.

### 2. Excessively Long Fade-in Animations

A slow fade-in on your main content is a direct hit to your LCP score. The browser's LCP timer doesn't stop until an element is fully visible. If you animate your hero image or headline with a slow ⁠opacity transition, you're forcing the browser to wait for that entire animation to finish before it can count the element as "painted." This introduces an artificial delay, adding the animation's full duration right on top of your LCP time.

### 3. LCP Being Hijacked by Delayed Modals or Overlays

You have to be careful with elements that load in late. A cookie banner or a newsletter pop-up that appears after the initial render can "hijack" the LCP if it's big enough. Your page's main headline might render in under a second, but if a huge banner becomes the largest thing on the screen at the 3-second mark, your LCP score is now 3 seconds. The best defense is to make sure these overlays are either small or are designed to appear as soon as possible so they don't interfere.

