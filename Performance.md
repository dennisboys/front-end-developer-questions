## Performance

Principle of performance optimisation:

- Reduce the number of files loaded in the browser
- Reduce the number of code executed in the browser

__Performance Benchmark__

Performance API (window.performance.timing)

```js
const timingInfo = window.performance.timing;

console.log({
  "TCP connection time": timingInfo.connectEnd - timingInfo.connectStart,
  "DNS lookup time": timingInfo.domainLookupEnd - timingInfo.domainLookupStart,
  TTFB: timingInfo.responseStart - timingInfo.navigationStart,
  "DOM ready": timingInfo.domContentLoadedEventStart - timingInfo.navigationStart,
  "DOM downloaded": timingInfo.responseEnd - timingInfo.responseStart
});
```

You can check the performance benchmark using [Lighthouse](https://www.npmjs.com/package/lighthouse), [PageSpeed Insight](https://developers.google.com/speed/pagespeed/insights/) or [WebPagetest](https://www.webpagetest.org/).

__Performance Optimization__

Following is a list of things you can do to optimize your web app.

- Look at the Network tab in developer tool to check which request takes the most of the time, so we can start from there.

- DNS prefetch `<link ref="dns-prefetch" href="//g.example.com" />`

- Use [minifier](https://www.minifier.org/) to minimize JS and CSS code, and also run Gzip on the minified files

- Minimize images
  - Use PNG for transparency, GIF for animation, JPEG for colorful images, SVG for icons, logos and illustrations
  - Use [TinyPNG](https://tinypng.com/) and [JPEG optimizer](http://jpeg-optimizer.com/) for compressing images
  - Lower JPEG image quality to about 30% - 60% ([ImageOptim](https://imageoptim.com/))
  - Resize image based on the size it will be displayed on the screen
  - Display different sized images for different screen sizes. Browser will ignore other images that are not for the current screen sizes

    ```
    @media screen and (min-width: 500px) {
      body {
        background: url('./medium-bg.jpg') no-repeat center center fixed;
        background-size: cover;
      }
    }

    @media screen and (min-width: 900px) {
      body {
        background: url('./large-bg.jpg') no-repeat center center fixed;
        background-size: cover;
      }
    }
    ```

  - Use images CDNs such as [imigx](https://www.imgix.com/)
  - Remove metadata of images using [verexif](http://www.verexif.com/)

- Reduce the number of files that need to be fetched using bundling technique

- Use above the fold CSS loading, separate the above-the-fold CSS style into a file and load it first

- Load style tag in the `<head>` and script right before `</body>`

- Use `<script async>` to load JS file asynchronously. These scripts should not modify DOM, such as Google Analytics script, because there is no guarantee when they will be loaded. Use `<script defer>` to runtm scripts after the HTML is parsed, scripts that are not that important and not required above the fold.

- Implement route-based code splitting, which utilizes `dynamic import()` under the hood to achieve the splitting.

- Avoid unnecessary rendering, for example, we can consider using `PureComponent` and implementing `shouldComponentUpdate` life cycle method in React to fine tune component rendering.

- Lazy load components in SPA.

- Use Webpack bundleAnalyzerPlugin to analyze your bundle.

- Do not use deep nested class selector (#test .class1 .class2), CSS selector resolves from right to left, deep mested selector would increase time for browser to generate a CSS tree.
