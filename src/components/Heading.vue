<template>
    <div id="nav">
      <h1>{{ heading }}</h1>
        <div>
            <router-link to="/">Home</router-link> |
            <router-link to="/about">About</router-link>
        </div>
    </div>
    
</template>

<script>
export default {
    props: {
        heading: String
    }
}
</script>

<style scoped>
    h1 {
        padding: 10px;
    }
    #nav {
        display: flex;
        flex-direction: column;
        background-color: cornflowerblue;
        color: white;
        align-items: center;
    }
    a {
        text-decoration: none;
        color: white;
    }
</style>