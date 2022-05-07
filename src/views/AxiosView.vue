<template>
    <div>
        <input type="text" v-model="search" @keyup.enter="callData">
        <button @click="callData">검색</button>

         <table>
                <tr>
                    <th>제목</th>
                    <th>이미지</th>
                </tr>
                <tr v-for="(item, index) in list" :key="index">
                    <td>
                        <a :href="item.doc_url" target='_blank'>
                            {{item.display_sitename}}
                        </a>
                    </td>
                    <td>
                        <img :src="item.thumbnail_url">
                    </td>
                </tr>
            </table>

    </div>
</template>

<script>
    import axios from "axios";
   // const REST_API_KEY='024714f232bb120a9fdc173e5c3064b7';
    export default {

        data : ()=> ({
            search : '',

            list : []
        }),

        methods : {
            callData(){
                axios.get(`https://dapi.kakao.com/v2/search/image?query=${this.search}&page=1&size=10&sort=recency`, {
                    headers : {
                        Authorization: `KakaoAK ${process.env.VUE_APP_KAKAO_KEY}`
                    }
                }).then(repsonse => {
                    console.log(repsonse);
                    this.list = repsonse.data.documents;
                }).catch(error => {
                    console.error(error);
                })
            },


        }
    }
</script>

<style scoped>

</style>