<template>
    <div class="container-fluid" id="cont">
        <div class="card" v-for="article in articles">
            <img :src="article.urlToImage" class="card-img-top" alt="">
            <div class="card-body">
                <p class="h4 card-title">{{article.title}}</p>
                <p class="card-text">{{article.description}}</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            articles: []
        }
    },
    created() {
        let self = this

        fetch('https://newsapi.org/v2/top-headlines?country=us', 
        {
            headers: {
                'Authorization': `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`
            }
        }).then((response)=> {
            return response.json()
        
        }).then((data)=> {
            console.log(data)
            self.articles = data.articles
        })
        }
}
</script>

<style scoped>
.container-fluid {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}

.card {
    max-width: 25%;
    margin: .5%;
    max-height: 10%;
    border-bottom: greenyellow inset;
}

.card-img-top {
    width: 100%;
    height: auto;
}

</style>