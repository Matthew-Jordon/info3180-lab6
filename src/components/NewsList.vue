<template>
<form class="d-flex flex-column justify-content-center" @submit.prevent="searchNews">
    <div class="input-group mx-sm-3 mb-2">
        <label class="visually-hidden" for="search">Search</label>
        <input type="search" name="search" v-model="searchTerm" id="search" class="form-control mb-2 mr-sm-2" placeholder="Enter search term here" />
        <button class="btn btn-primary mb-2">Search</button>
    </div>
    <p>You are searching for {{ searchTerm }}</p>
 </form>
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
            articles: [],
            searchTerm: ''
        }
        
    },
    methods: {
        searchNews() {
        let self = this;
        fetch(`https://newsapi.org/v2/everything?q=${self.searchTerm}&language=en`, {
            headers: {
                'Authorization': `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`,
                }
            }).then(function(response) {
                return response.json();
            }).then(function(data) {
                console.log(data);
                self.articles = data.articles;
            });
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