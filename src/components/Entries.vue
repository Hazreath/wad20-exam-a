<template>
    <div class="main-container">
        <!-- #Tasks Error : by default, Newest to oldest, so it needs to display the other sort option -->
        <button id="sort-button" @click="sortEntries()">Oldest to newest</button>
        <div class="test" v-for="entry in sortedEntries" :key="entry.id">
            <span>{{entry.title}},</span>
        </div>
        <div class="entries" v-for="entry in sortedEntries" :key="entry.id">
            <!-- <h3>{{this.newestToOldest}}</h3> -->
            <h1>{{entry.title}}</h1>
            <p>{{entry.date | readableDate}}</p>
            <!-- FOR TEST PURPOSES -->
            <!-- <p>{{entry.date}}</p> -->
            <img :src="entry.image" />
            <p>{{entry.text}}</p>
        </div>
    </div>
</template>


// id: 2,
//                         title: "My entry #2",
//                         date: "2020-12-31 14:15:16",
//                         image: "https://picsum.photos/id/1015/200/150",
//                         text: 
<script>
    export default {
        name: 'Entries',
        props: {
            entries: Array
        },
        data: function() {
            return {
                newestToOldest : false
            }
            
        },
        computed: {
            sortedEntries: {
                get() {
                    let s = this.entries
                    return s.sort(this.compareDate).reverse()
                }
            }
        },
        methods: {
            compareDate: function(a,b) {
                return a.date == b.date ? 0 : a.date > b.date ? 1 : - 1
            },

            sortEntries: function() {
                var button = document.getElementById("sort-button")
                this.newestToOldest = !this.newestToOldest

                this.sortedEntries.reverse()
                // alert(this.newestToOldest)
                if (this.newestToOldest) {
                    button.innerText = "Oldest to newest"
                } else {
                    button.innerText = "Newest to Oldest"
                }
            }
        },

        filters: {
            readableDate: function(val) {
                let date = new Date(val)

                let weekDays = [
                    "Monday",
                    'Tuesday',
                    'Wednesday',
                    'Thursday',
                    'Friday',
                    'Saturday',
                    'Sunday'
                ]

                let months = [
                    "January",
                    "February",
                    "March",
                    "April",
                    "May",
                    "June",
                    "July",
                    "August",
                    "September",
                    "October",
                    "November",
                    "December"
                ]
                
                let hour = date.getHours() == 12 ? 12 : date.getHours() % 12
                let morningOrAfternoon = date.getHours() <= 12 ? "AM" : "PM"
                let str = weekDays[date.getDay()] + ", " 
                    + months[date.getMonth()] + " "
                    + date.getDate() + ", "
                    + date.getFullYear() + " "
                    + hour + ":" + date.getMinutes() + " "
                    + morningOrAfternoon
                    
                return str
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .main-container {
        text-align:center;
    }
    h3 {
        margin: 40px 0 0;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    li {
        display: inline-block;
        margin: 0 10px;
    }

    a {
        color: #42b983;
    }
</style>
