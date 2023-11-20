## Create the back-end
1. Create a new space in the Storyblok app

## Create the front-end
1. Create a new project
```
npx nuxi@latest init <project-name>
cd <project-name>
npm install
npm run dev
```

## Fetch API

1. In `pages>index.vue`: initialize variable to store data; use a fetch request to get the data from storyblok.
```
<template>
    <h1>index.vue</h1>
</template>

<script>
  export default {
      data: function(){
          return{
              body:null
          }
      },
      created: function() {
          fetch('...')
          .then(resp => resp.json())
          .then(data => this.body = data.story.content.body)
      },
    //   computed: {
    //         dataBanner: function(){
    //             return this.body.find(data => data.component === "Banner")
    //         }
    //     }
  }
</script>
```
2. Delete `app.vue`

## Render data
1. Create component components>Banner.vue
```
<template>
    <h2>Banner component</h2>
        
    <header class="space-header" :style="{ backgroundImage: 'url(' + body.image.filename + ')' }">
    
        <div class="header-content">
            <h1>{{ body.headline }}</h1>
            <p class="subtitle"> {{ body.subheadline }} </p>
            <NuxtLink to="https://nuxtjs.org" class="explore-button">Read more</NuxtLink>
        </div>
    </header>
</template>

<script>
    export default {
        props:['body']
    }
</script>

<style>
    .space-header {
    background-repeat: no-repeat;
    background-position: center center;
    background-size: cover;
    height: 50vh; 
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    text-align: center;
    }

    .header-content {
    max-width: 600px; 
    }

    .space-header h1 {
    font-size: 3rem; 
    margin-bottom: 0.5rem;
    }

    .subtitle {
    font-size: 1.5rem;
    margin-bottom: 2rem;
    }

    .explore-button {
    padding: 10px 20px;
    border: 2px solid white; 
    border-radius: 50px; 
    text-transform: uppercase;
    text-decoration: none;
    color: white;
    font-size: 1rem;
    transition: background-color 0.3s, color 0.3s;
    }

    .explore-button:hover {
    background-color: white;
    color: #333; 
    text-decoration: none;
    }

</style>
```

2. Call the component
```
<LandingBanner v-if="body" :body="dataBanner" />
```

## Multiple elements
1. Loop the body and use the conditional render to display the components
```
<template>
    <div v-if="body" v-for="block in body" :key="block._uid" >
        <Banner v-if="block.component === 'Banner'" :body="block" />
    </div>
</template>
```


## Fetch Wordpress
1. In `pages>index.vue`, change the access URL and render a list of posts
```
fetch('https://wptavern.com/wp-json/wp/v2/posts?_fields=author,id,excerpt,title,link')
```
```
<ol>
    <div v-for="post in posts" :key="post.id">
        <li>
            <NuxtLink :to="`/post/${post.id}`" ><h2 v-html="post.title.rendered"></h2></NuxtLink >
            <p v-html="post.excerpt.rendered"></p>
        </li>
    </div>
</ol>
```
2. Create the blog component pages/post/[id].vue
```
<template>
    <div class="post-container">
        <h1 v-if="post.title">{{ post.title.rendered }}</h1>
        <div v-if="post.content" v-html="post.content.rendered"></div>
    </div>
</template>

<script>

export default{
    data: function(){
        return{
            id: this.$route.params.id,
            post:''
        }
    },
    created: function(){
        fetch(`https://wptavern.com/wp-json/wp/v2/posts/${this.id}?_fields=author,id,content,title,link`)
          .then(resp => resp.json())
          .then(data => this.post = data )
    }
}

</script>

<style>
img{
    max-width: 100%;
}
.post-container{
    max-width: 800px;
    margin: 0 auto;
}
</style>
```
