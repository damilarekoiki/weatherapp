<template>
    <div id="app"  :style="{
        'background-image': 'url('+require('./assets/backgrounds/'+bgImage)+')',
        'background-size': 'cover',
        'background-position': 'bottom',
        'transition': '0.4s'
        }">
        <main>

            <div class="search-box">
                <input 
                v-model="query" @keypress.enter="fetchWeather" type="text" class="search-bar" placeholder="Search..." />
            </div>

            <div class="weather-wrap" v-if="typeof weather.main!= 'undefined'">
                <div class="location-box">
                    <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
                    <div class="date">{{ dateBuilder }} </div>
                </div>
                <div class="weather-box">
                    <div class="temp">{{ Math.round(weather.main.temp) }}&#176;C</div>
                    <div class="weather">{{ weather.weather[0].main}}</div>
                </div>
            </div>

        </main>
    </div>
</template>


<script>
export default {
    data(){
        return {
            api_key: '1016bfba2269bf956e1ba5438bf8c7f1',
            url_base: 'http://api.openweathermap.org/data/2.5/',
            query: '',
            weather: {},
            backgrounds: [],
            weathers: [],
            bgImage: 'cold-bg.jpg',
        }
    },
    methods:{
        fetchWeather(){
            fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
            .then(res => {
                console.log(res)
                return res.json();
            }).then( this.setResults )
        },

        setResults(results){
            this.backgrounds=[
                "thunderstorm.jpg","rain.jpg","rain.jpg","snow.jpg","mist.jpg","smoke.jpg","mist.jpg",
                "sand.jpg","fog.jpg","sand.jpg","volcano.jpg","thunderstorm.jpg","tornado.jpg","clear.jpg","clouds.jpg"
            ];
            this.weathers=[
                "thunderstorm","drizzle","rain","snow","mist","smoke",
                "haze","dust","fog","sand","ash","squall","tornado","clear","clouds"
            ]

            this.bgImage=this.backgrounds[this.weathers.indexOf( (results.weather[0].main).toLowerCase() )];

            this.weather=results;

        },

    },
    computed: {
        dateBuilder(){
            let d = new Date();

            let months = ['January','February','March','April','May','June','July','August','September','October','November','December'];
            let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];

            let day=days[d.getDay()];
            let date = d.getDate();
            let month = months[d.getMonth()];
            let year = d.getFullYear();

            return `${day} ${date} ${month} ${year}`
        }
    }
}
</script>

<style scoped>
    *{
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    body{
        font-family: 'montserrat', sans-serif;
    }

    main{
        min-height: 100vh;
        padding: 25px;

        background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75) );
    }
    .search-box{
        width: 100%;
        margin-bottom: 30px;
    }

    .search-box .search-bar{
        display: block;
        width: 100%;
        padding: 15px;

        color: #313131;
        font-size: 20px;
        appearance: none;
        border: none;
        outline: none;
        background: none;

        box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
        background-color: rgba(255, 255, 255, 0.5);
        border-radius: 0px 16px 0px 16px;
        transition: 0.4s;
    }
    .search-box .search-bar:focus{
        box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
        background-color: rgba(255, 255, 255, 0.75);
        border-radius: 16px 0px 16px 0px;
    }

    .location-box .location {
        color: #fff;
        font-size: 32px;
        font-weight: 500;
        text-align: center;
        text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
    }

    .location-box .date{
        color: #fff;
        font-size: 20px;
        font-weight: 300;
        font-style: italic;
        text-align: center;
    }

    .weather-box{
        text-align: center;
    }

    .weather-box .temp{
        display: inline-block;
        padding: 10px 25px;
        color: #fff;
        font-size: 102px;
        font-weight: 900;
        
        text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
        background-color: rgba(255, 255, 255, 0.25);
        border-radius: 16px;
        margin: 30px 0px;

        box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    }

    .weather-box .weather{
        color: #fff;
        font-size: 48px;
        font-weight: 700;
        font-style: italic;
        text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    }
</style>



















