<template>
    <div class="main-container">
        <!-- #Tasks Error : by default, Newest to oldest, so it needs to display the other sort option -->
        <button id="sort-button" @click="sortEntries()">Oldest to newest</button>
        
        <!-- Initial sort -->
        <input type="hidden" v-if="this.firstVisit" :value="this.initialSort()">
        
        <!-- FOR TEST PURPOSES -->
        <!-- <h3>{{this.newestToOldest}}</h3> -->
        <!-- <h3>{{this.initsort}}</h3> -->
        
        <div class="entries" v-for="entry in entries" :key="entry.id">
            
            <h1>{{entry.title}}</h1>
            <p>{{entry.date | readableDate}}</p>
            <!-- FOR TEST PURPOSES -->
            <!-- <p>{{entry.date}}</p> -->
            <img :src="entry.image" />
            <p>{{entry.text}}</p>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Entries',
        props: {
            entries: Array
        },
        data: function() {
            return {
                newestToOldest : true,
                initsort : 0, // Debug purposes
                firstVisit : true
            }
            
        },
        computed: {
            sortedEntries: {
                get() {
                    let s = this.entries
                    return s.sort(this.compareDate).reverse()
                },
            },
            
            
        },
        methods: {
            initialSort: function() {
                this.firstVisit = false
                this.initsort += 1
                let s = this.sortedEntries
                this.entries = s.sort(this.compareDate).reverse()
            },
            compareDate: function(a,b) {
                return a.date == b.date ? 0 : a.date > b.date ? 1 : - 1
            },

            sortEntries: function() {
                var button = document.getElementById("sort-button")
                this.newestToOldest = !this.newestToOldest
                if (this.newestToOldest) {
                    this.entries = this.sortedEntries
                    button.innerText = "Oldest to newest"
                    
                } else {
                    this.entries = this.sortedEntries.reverse()
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
