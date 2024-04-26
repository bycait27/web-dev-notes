# serving static assets
**static asset:** all of the non-dynamic files (images, fonts, icons, compiled css, js files)

the easiest way to serve static assets is by using the default /public folder provided by next.js
- every file inside this folder is considered and served as a static asset

**if we put the following index.txt file inside the /public folder, the text "Hello, world!" will be displayed in the browser:**

```
Hello, world!
```

## reducing cumulative layout shift through image optimization
the **image file** can critically affect our website performance and seo

most of the time, serving non-optimized images will worsen our user experience because they may take some time to load, and once they do, they'll move part of the layout after the rendering
- referred to as **cumulative layout shift (cls)**
![cumulative layout shift](../assets/cumulative-layout-shift.png "cumulative layout shift")
    - this can cause many problems in terms of ux

## next.js automatic image optimization
next.js introduced a helpful Image component and automatic image optimization to combat cls

automatic image optimization will take care of serving our images using modern formats (webp) to all those browsers that support it
- also provides fallback for older image formats (png or jpeg) in case the browser doesn't support it
- resizes our images to avoid serving heavy pictures to the client 
    - this would negatively affect the asset's download speed

automatic image optimization works on demand 
- optimizes, resizes, and renders the image only when the browser has requested it
    - this will work with any external data source (any cms image service, such as unsplash or pexels)
    - it won't slow down the build phase

**for example, we could serve an image coming from unsplash by configuring our next.config.js file and using the Image component**

**next.config.js file:**

```
module.exports = {
  images: {
    domains: ['images.unsplash.com'],
  },
};
```

now, every time we use an image coming from that hostname inside an Image component, next.js will automatically optimize it for us

**imagepage.js file:**

```
import Image from "next/image";

function IndexPage() {
  return (
    <div>
      <Image
        src="https://images.unsplash.com/photo-1605460375648-278bcbd579a6"
        width={500}
        height={200}
        alt="A beautiful English Setter"
        priority={true}
      />
    </div>
  );
}

export default IndexPage;
```

we can crop our image to fit the desired dimensions by using the optional layout prop
- **accepts four different values:** fixed, intrinsic, responsive, fill

**fixed:** it works just like the img html tag. if we change the viewport size, it will keep the same size (won't provide a responsive image for smaller or bigger screens)

**responsive:** it works in the opposite way as fixed. as we resize our viewport, it will serve differently optimized images for our screen size

**intrinsic:** it's halfway between fixed and responsive. it will serve different image sizes as we resize down our viewport, but it will leave the largest image untouched on bigger screens

**fill:** it will stretch the image according to its parent element's width and height. however, we can't use fill alongside the width and height props. we can use fill or width and height

**here is an example of the refactored code of imagepage.js using these props:**

```
import Image from "next/image";

function IndexPage() {
  return (
    <div>
      <div style={{ width: 500, height: 200,position: "relative",}}>
        <Image
          src="https://images.unsplash.com/photo-1605460375648-278bcbd579a6"
          layout="fill"
          objectFit="cover"
          alt="A beautiful English Setter"
          priority={true}
        />
      </div>
    </div>
  );
}

export default IndexPage;
```

we wrapped the Image component with a fixed size div and the css position property set to relative. the width and height props were also removed from our Image component because it will stretch following its parent div sizes

the objectFit prop was also added and set to cover so that it will crop the image according to its parent div size 

upon inspection of the image in the web browser, the Image component has generated many different image sizes that are served using the srcset property of a standard img html tag
- we will also see that the image is served as webp, even though the original image served from unsplash was a jpeg
    - this is because the browser supports this format...if we were to use a different browser that doesn't support webp format, it will be served as a jpeg

*the whole optimization process occurs on the server where next.js is running
- if our app contains tons of images, it could affect our server performance

## implementing image optimization on external services 
by default, automatic image optimization runs on the same server as next.js
- if we are running our website on a small server with low resources, this could potentially affect its performance
    - **next.js allows us to run automatic image optimization on external services by setting the loader option inside our next.config.js file:** 

    ```
    module.exports = {
        images: {
            loader: 'akamai',
            domains: ['images.unsplash.com']
        }
    };
    ```

if we're deploying our web app to vercel, we don't actually need to set up any loader in our next.config.js file because vercel will take care of optimizing and serving the image files for us

**if we want to use our custom image optimization server, we can use the loader prop directly inside our component:**

```
import Image from "next/image";

const loader = ({ src, width, quality }) => {
  return `https://example.com/${src}?w=${width}&q=${quality || 75}`;
};

function CustomImage() {
  return (
    <Image
      loader={loader}
      src="/myimage.png"
      alt="My image alt text"
      width={350}
      height={540}
    />
  );
}
```

we can serve images coming from any external service, allowing us to take advantage of custom image optimization servers
- every service has its own apis for resizing and serving images
    - before creating a custom loader, make sure to read the documentation of your image optimization server

it is important to correctly serve images, as it can affect our user experience in many critical ways
- next.js makes it quite effortless thanks to its built-in components and optimizations
