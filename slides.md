---
# try also 'default' to start simple
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: '#1f222a'
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
# page transition
transition: slide-left

fonts:
  sans: 'Plus Jakarta Sans'
  mono: JetBrains Mono
---

# Efficiently caching Geospatial data

An exploration of different techniques we can use to cache geolocations.


<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/SphericalKat/demystifying-webrtc" target="_blank" alt="GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
layout:default
---

# Background

- Latitude and longitude are the primary parameters
- Traditional caching techniques fall flat

## Example query

<br />

```
GET /weather?latitude=40.741111&longitude=-73.987448
```

<!-- 

 -->

---
transition: fade-out
layout: default
---

# Let's define some metrics

- **Latency**: The simplest and most widely known. Time taken for a response to be returned to the caller.
- **Cache hit ratio**: Useful to measure the effectiveness of our caching strategy. Defined as the percentage of incoming requests that were found in the cache

<br/>

## Problems
- Too **granular**. Two locations a few metres apart can have slightly different values for lat & lon.
- Even with a high volume of requests, it is unlikely that multiple requests are made with the exact same coordinates.

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/guide/syntax#embedded-styles
-->

<style>

</style>

---
transition: fade-out
layout: default
---

# The Naive approach

- Use in-memory caches such as Redis or Memcached
- Easy and widely known

<br/>

### Problems
- Too **granular**. Two locations a few metres apart can have slightly different values for lat & lon.
- Even with a high volume of requests, it is unlikely that multiple requests are made with the exact same coordinates.

---
---
## Results
- Cache hit ratio: 0.0%
- P50 latency:  957.9884999999999 ms
- P75 latency 958.805 ms
- P95 latency 966.2163999999999 ms
- P99 latency 970.86328 ms

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/guide/syntax#embedded-styles
-->

<style>

</style>

<!--

-->

---
transition: slide-up
---

# Quadtrees

- Recursively divide a 2 dimensional space into quadrants.
- Similar to binary trees. Each node has exactly 4 children (or no children for leaf nodes)
- Each leaf node has a maximum capacity; upon reaching the maximum capacity, the bucket splits into 4 further leaf nodes.

<!--
A node of a point quadtree is similar to a node of a binary tree, with the major difference being that it has four pointers (one for each quadrant) instead of two ("left" and "right") as in an ordinary binary tree. Also a key is usually decomposed into two parts, referring to x and y coordinates. Therefore, a node contains the following information:
-->

<img src="https://github.com/amay12/SpatialSearch/raw/master/images/PRexamp.png" />

---
---
# Results
- Cache hit ratio: 90.0%
- P50 latency: 204.5125 ms
- P75 latency: 230.06475 ms
- P95 latency: 542.818849999999 ms
- P99 latency: 895.8301700000002 ms

---
---
# K-d trees (K-dimensional trees)
- Special case of binary tree. Data in each node is a point in a K-dimensional space
- Affected by the curse of dimensionality, not a problem since at most we will use 3 dimensions

## Specifics of my implementation
- Each lat/long pair is converted to x,y,z coordinates on a 3D unit sphere (So a K=3)
- Distances are calculated as simple euclidean square distances and converted to Kilometres as required

---
---


![label](https://res.cloudinary.com/practicaldev/image/fetch/s--0svT9Fp8--/c_imagga_scale,f_auto,fl_progressive,h_900,q_auto,w_1600/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ljpizoxvwhcocys863lw.PNG)
<!--

 -->

---
---

# Results
- Cache hit ratio: 99.6%
- P50 latency: 178.64800000000002 ms
- P75 latency: 186.75349999999997 ms
- P95 latency: 203.50105 ms
- P99 latency: 212.73934 ms



