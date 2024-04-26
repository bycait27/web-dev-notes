# grouping common meta tags
it is common to create one or more components (depending on our needs) to handle most of the common head meta tags

if we wanted to add support to our blog site for open graph data, twitter cards, and other metadata for our blog posts, we could groupthis common data inside a PostHead component

**let's create a PostHead.js file in our components folder:**

```
import Head from "next/head";

function PostMeta(props) {
  return (
    <Head>
      <title>{props.title}</title>
      <meta name="description" content={props.subtitle} />
      {/* open-graph meta */}
      <meta property="og:title" content={props.title} />
      <meta property="og:description" content={props.subtitle} />
      <meta property="og:image" content={props.image} />
      {/* twitter card meta */}
      <meta name="twitter:card" content="summary" />
      <meta name="twitter:title" content={props.title} />
      <meta name="twitter:description" content={props.description} />
      <meta name="twitter:image" content={props.image} />
    </Head>
  );
}
```

**now we need to make a mock for our posts. let's add a new folder called data and a file called posts.js inside it:**

```
export default [
  {
    id: "qWD3Pzce",
    slug: "dog-of-the-day-the-english-setter",
    title: "Dog of the day: the English setter",
    subtitle:'The English Setter dog breed was named for these dogs\' practice of "setting," or crouching low, when they found birds so hunters could throw their nets over them',
    image: "https://images.unsplash.com/photo-1605460375648-278bcbd579a6",
  },
  {
    id: "yI6BK404",
    slug: "about-rottweiler",
    title: "About rottweiler",
    subtitle:"The rottweiler is a breed of domestic dog, regarded as medium-to-large or large. The dogs were known in German as Rottweiler Metzgerhund, meaning Rottweil butchers' dogs, because their main use was to herd livestock and pull carts laden with butchered meat to market",
    image: "https://images.unsplash.com/photo-1567752881298-894bb81f9379",
  },
  {
    id: "VFOyZVyH",
    slug: "running-free-with-collies",
    title: "Running free with collies",
    subtitle:"Collies form a distinctive type of herding dogs, including many related landraces and standardized breeds. The type originated in Scotland and Northern England. Collies are medium-sized, fairly lightly built dogs, with pointed snouts. Many types have a distinctive white color over the shoulders",
    image: "https://images.unsplash.com/photo-1517662613602-4b8e02886677",
  },
];
```

now we just need to create a [slug] page to display our posts. let's create a new file called [slug].js inside the pages/blog directory:**

```
import PostHead from '../../components/PostHead';
import posts from '../../data/posts';
export function getServerSideProps({ params }) {
  const { slug } = params;
  const post = posts.find((p) => p.slug === slug);
  return {
    props: {
      post
    }
  };
}
function Post({ post }) {
return (
  <div>
    <PostHead {...post} />
    <h1>{post.title}</h1>
    <p>{post.subtitle}</p>
  </div>
);
}
export default Post;
```

*logically separating head-related coomponents from other components leads to a more organized code base