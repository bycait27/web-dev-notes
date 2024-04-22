# static site generation (ssg)
when we need to build a dynamic page and seo isn't really important (admin pages, private profile pages, etc.)...why don't we just ssend a static page to the client and load all the data once the page has been transferred to the browser?

with static site generation (ssg), we will be able to pre-render some specific pages (or even the whole website if necessary) at build time
- when we are building our web app, there might be some pages that won't change their content very often (makes sense for us to serve them as static assets)
- next.js will render these pages during the build phase and will always serve that specific html that will become interactive after react hydration (like ssr)

## advantages of ssg
**easy to scale:** static pages are just html files that can be served and cached easily by and cdn
- even if we use our own web server, there is a very low workload (given that no hard computations are needed for serving a static asset)

**outstanding performances:** html is pre-rendered at build time, so both the client adn server can bypass the runtime rendering phase for each request. the web server will send the static file and the browser will just display it
- no data fetching is required on the server side (already pre-rendered on the static html markup)
    - reduces the potential latency for each request

**more secure requests:** we don't need to send any sensitive data to the web server for rendering the page
- no access to apis, databases, or other private info is required
    - every piece of info needed is already part of the pre-rendered page

## limitations of ssg
the biggest concern with ssg is that once the page has been built, the content will remain the same until the next deployment

**with other providers,**if we misspell a word, we would need to rebuild the whole website to change just a work (need to repeat the data fetching and rendering phase at build time)

***remember:** statically generated pages are created at build time and served as static assets for each request

## incremental static regeneration (isr)
next.js provides a unique approach for solving the problem mentioned above
- incremental static regeneration (isr)

isr allows us to specify at the page level how long next.js should wait before re-rendering a static page updating its content

**for example:** we want to build a page showing some dynamic content, but the data fetching phase takes too long to succeed...
- this would lead to bad performance and would give users a bad experience
- we can use a combo of ssg and isr to solve this problem (hybrid approach)

**consider a rest api request for some data is taking up a few seconds to succeed...we can cache this data for up to 10 minutes using ssg and isr (data won't change a lot during this time):**

```
import fetch from 'isomorphic-unfetch';
import Dashboard from './components/Dashboard';

// used at build time by next.js for getting the data and
// rendering the page 
export async function getStaticProps() {
  const userReq = await fetch('/api/user');
  const userData = await userReq.json();
  const dashboardReq = await fetch('/api/dashboard');
  const dashboardData = await dashboardReq.json();

  return {
    props: {
      user: userData,
      data: dashboardData,
    },
    revalidate: 600 // time in seconds (10 minutes)
  };
}

function IndexPage(props) {
  return (
    <div>
      <Dashboard user={props.user} data={props.data} />
    </div>
  );
}

export default IndexPage;
```

getStaticProps() won't be called again until the next build
- **comes at a cost:** if we want to update the page content, we have to rebuild the entire website

**to avoid this problem,** next.js has the revalidate property
- this can be set inside the returning object of our getStaticProps function and indicates after how many seconds we should rebuild the page once a new request arrives

**with the use of the revalidate property, next.js will behave as follows:**

1. next.js fills the page with the results of getStaticProps at build time, statically generating the page during the build process

2. in the first 10 minutes, every user will access the exact same static page

3. after 10 minutes, if a new request occurs, next.js will server-side render that page, re-execute the getStaticProps function, and save and cache the newly rendered page as a static asset, overriding the previous one created at build time

4. every new request, within the next 10 minutes, will be served with that new statically generated page

***if no requests occur after 10 minutes, next.js won't rebuild its pages**

once our website has been deployed, we'll have to wait the length of the expiration time set in the revalidate option for the page to be rebuilt

***static site generation is a great way to create fast and secure web pages, but we might want to have more dynamic content**
- with next.js we can decide which page should be rendered at build time (ssg) or request time (ssr)
    - we can take the best of both approaches by using ssg and isr together

