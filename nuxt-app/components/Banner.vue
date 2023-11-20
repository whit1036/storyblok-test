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