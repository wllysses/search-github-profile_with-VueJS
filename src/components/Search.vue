<template>
    <div class="search">
        <div class="search-box">
            <i class="fa-brands fa-github"></i>
            <h2>Buscar Perfil</h2>
            <input type="text" placeholder="Nome do usuário" v-model="nome" @keyup.enter="getInfoUsers"/>
            <button @click="getInfoUsers"><i class="fa-solid fa-magnifying-glass"></i></button>
        </div>
        <div class="results" v-show="userName">
            <div class="info">
                <img :src="userAvatar" alt="Foto perfil">
                <div class="userInfos">
                    <h3>{{ userName }}</h3>
                    <p>{{ userBio }}</p>
                </div>
                <div class="stats">
                    <div>
                        <h4>👥 Seguidores</h4>
                        <span>{{userFollowing}}</span>
                    </div>
                    <div>
                        <h4>👥 Seguindo</h4>
                        <span>{{ userFollowers }}</span>
                    </div>
                    <div>
                        <h4>📂 Repositórios</h4>
                        <span>{{ userRepositories }}</span>
                    </div>
                </div>
                <div class="repositories">
                    <h3>Repositórios</h3>
                    <ul>
                        <li v-for="(item, index) in reposName" :key="index">
                            <a :href="item.html_url" target="_blank">{{item.name}}</a>
                            <div class="repos-stats">
                                <div>
                                    <i class="fa-solid fa-code-fork"></i> {{item.forks_count}}
                                </div>
                                <div>
                                    <i class="fa-solid fa-star"></i> {{item.stargazers_count}}
                                </div>
                                <div>
                                    <i class="fa-solid fa-eye"></i> {{item.watchers_count}}
                                </div>
                                <div>
                                    <i class="fa-solid fa-code"></i> {{item.language}}
                                </div>
                            </div>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
    </div>    
</template>

<script>

export default {
    name: 'mySearch',
    data() {
        return {
            nome:'',
            userAvatar: '',
            userName: '',
            userBio: '',
            userFollowing: '',
            userFollowers: '',
            userRepositories: '',
            reposName: [],
        }
    },
    methods: {
        getInfoUsers() {
            let url = `https://api.github.com/users/${this.nome}`
            fetch(url)
            .then(response => {return response.json()})
            .then(json => {
                if(this.nome.length === 0) {
                    alert('INFORME UM NOME PARA O USUÁRIO.')
                    return
                }

                if(json.message === 'Not Found') {
                    alert('ESTE USUÁRIO NÃO EXISTE.')
                    this.nome = ''
                    return
                }

                this.nome = ''
                this.userAvatar = json.avatar_url
                this.userName = json.name ?? 'O usuário não possui nome cadastrado'
                this.userBio = json.bio ?? 'O usuário não possui bio cadastrada'
                this.userFollowing = json.following
                this.userFollowers = json.followers
                this.userRepositories = json.public_repos
            })

            let urlRepos = `https://api.github.com/users/${this.nome}/repos?per_page=5`
            fetch(urlRepos)
            .then(response => {return response.json()})
            .then(json => {
                this.reposName = []
                json.forEach(repos => {
                    this.reposName.push(repos)
                })
            })
        }
    }
}
</script>

<style scoped>
.search {
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.39);
    border-radius: 10px;
    background-color: white;
    max-width: 400px;
    width: 95%;
    min-height: 150px;
    margin: auto;
    display: flex;
    align-content: center;
    flex-direction: column;
    justify-content: center;
}

.search-box {
    width: 100%;
    height: 100%;
    height: 100%;
    text-align: center;
    padding: 10px;
}

.search-box .fa-brands.fa-github {
    font-size: 30px;
}

.search-box h2 {
    font-size: 20px;
    margin: 10px 0;
}

.search-box input[type="text"] {
    height: 30px;
    border-radius: 20px;
    padding-left: 10px;
    border: 1px solid black;
    outline: none;
}

.search-box button {
    width: 30px;
    height: 30px;
    border-radius: 50%;
    border: none;
    background-color: black;
    color: white;
    margin-left: 5px;
    cursor: pointer;
}

.search .results {
    text-align: center;
    margin-top: 30px;
}

.search .results .info img {
    width: 150px;
    border-radius: 50%;
}

.search .results .info .userInfos {
    margin-top: 20px;
}

.search .results .info .userInfos p {
    font-size: 12px;
    width: 200px;
    margin: 0 auto;
}

.search .results .info .stats {
    width: 80%;
    margin: 10px auto;
    display: flex;
    align-items: center;
    justify-content: space-around;
}

.search .results .info .stats h4 {
    font-size: 12px;
}

.search .results .repositories ul li {
    padding: 5px;
    background-color: #7d1dd8;
    border-radius: 10px;
    width: 90%;
    margin: 10px auto;
    color: white;
    transition: 0.2s;
}

.search .results .repositories ul li:hover {
    background-color: #6518ad;
    box-shadow: 0px 5px 8px rgba(0, 0, 0, 0.322);
}

.search .results .repositories ul li a {
    color: white;
}

.search .results .repositories .repos-stats {
    margin-top: 5px;
    display: flex;
    justify-content: space-evenly;
}
</style>