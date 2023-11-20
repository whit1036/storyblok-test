<!-- <template>
    <h1>index.vue</h1>
    {{ body }}
    <Banner v-if="body" :body="dataBanner" />
</template> -->

<template>
    <div v-if="body" v-for="block in body" :key="block._uid" >
        <Banner v-if="block.component === 'Banner'" :body="block" />
    </div>
</template>

<script>
  export default {
      data: function(){
          return{
              body:null
          }
      },
      created: function() {
          fetch('https://api-us.storyblok.com/v2/cdn/stories/home?version=draft&token=QWgwpqA0SB7kAMoeFVf2NQtt&cv=1700504183')
          .then(resp => resp.json())
          .then(data => this.body = data.story.content.body)
      },
      computed: {
            dataBanner: function(){
                return this.body.find(data => data.component === "Banner")
            }
        }
  }
</script>