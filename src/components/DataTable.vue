<template>
    <div class="table">
        <div>
            <table @contextmenu.prevent="openMenu">

                <tr class="clickable">
                    <th @click="sortList('city')" class="cities">City</th>
                    <th @click="sortList('day_min')" class="mins">MinTemp</th>
                    <th @click="sortList('day_max')" class="maxs">MaxTemp</th>
                </tr>
                <tr class="navigation clickable">
                    <td @click="sortList('city')" class="city cities chosen">
                        <span v-show="chosenColumn === 'city'"
                            :class="[!sortedbyASC ? 'mirrorVertSpan' : 'normalVertSpan']">V</span>
                        by city
                        <span v-show="chosenColumn === 'city'"
                            :class="[!sortedbyASC ? 'mirrorVertSpan' : 'normalVertSpan']">V</span>
                    </td>
                    <td @click="sortList('day_min')" class="day_min mins"><span v-show="chosenColumn === 'day_min'"
                            :class="[!sortedbyASC ? 'mirrorVertSpan' : 'normalVertSpan']">V</span>
                        by min <span v-show="chosenColumn === 'day_min'"
                            :class="[!sortedbyASC ? 'mirrorVertSpan' : 'normalVertSpan']">V</span></td>
                    <td @click="sortList('day_max')" class="day_max maxs"><span v-show="chosenColumn === 'day_max'"
                            :class="[!sortedbyASC ? 'mirrorVertSpan' : 'normalVertSpan']">V</span>
                        by max <span v-show="chosenColumn === 'day_max'"
                            :class="[!sortedbyASC ? 'mirrorVertSpan' : 'normalVertSpan']">V</span></td>
                </tr>
                <tr v-for="(item, index) in collected" :key="index">
                    <td @dblclick="removeFromGeo" class="cities removeAble">{{ item.city }}</td>
                    <td @dblclick="removeFromGeo" class="mins removeAble">{{ item.day_min }}</td>
                    <td @dblclick="removeFromGeo" class="maxs removeAble">{{ item.day_max }}</td>
                </tr>
                <tr>
                    <td @click="addCity" class="clickable"><b> + </b></td>
                    <td @click="addCity" class="clickable">...</td>
                    <td @click="addCity" class="clickable">...</td>
                </tr>
                <tr v-show="adding">
                    <td class="hovered"><b>City</b></td>
                    <td class="hovered"><b>Latitude</b></td>
                    <td class="hovered">
                        <d>Longitude</d>
                    </td>
                </tr>
                <tr v-show="adding" v-for="(item, index) in add" :key="index" @click="addToGeo($event)">
                    <td class="hovered addAble">{{ item.city }}</td>
                    <td class="hovered addAble">{{ item.lat }}</td>
                    <td class="hovered addAble">{{ item.lon }}</td>
                </tr>
            </table>
        </div>
        <div>
        </div>
    </div>
</template>

<script>

import data from "@/assets/data";

export default {
    name: "DataTable",
    components: {
    },
    data() {
        return {
            geo: data.geo,
            add: data.add,
            collected: [],
            sortedbyASC: true,
            adding: false,
            chosenColumn: 'city',
        };
    },
    watch: {
        geo: {

            deep: true,
            handler: function (newValue) {

                const url = "https://api.open-meteo.com/v1/forecast?";
                const params = "&timezone=auto&daily=temperature_2m_max,temperature_2m_min";

                this.collected = [];

                for (let index in newValue) {

                    fetch(
                        url +
                        `latitude=${newValue[index].lat}&longitude=${newValue[index].lon}` +
                        params
                    )
                        .then((res) => res.json())
                        .then((data) => {
                            let obj_item = {};
                            obj_item.city = newValue[index].city;
                            obj_item.day_min = Math.min(...data.daily.temperature_2m_min);
                            obj_item.day_max = Math.max(...data.daily.temperature_2m_max);
                            this.collected.push(obj_item);
                        })
                }
            }
        }
    },
    mounted() {

        const url = "https://api.open-meteo.com/v1/forecast?";
        const params = "&timezone=auto&daily=temperature_2m_max,temperature_2m_min";

        for (let index in this.geo) {

            fetch(
                url +
                `latitude=${this.geo[index].lat}&longitude=${this.geo[index].lon}` +
                params
            )
                .then((res) => res.json())
                .then((data) => {
                    let obj_item = {};
                    obj_item.city = this.geo[index].city;
                    obj_item.day_min = Math.min(...data.daily.temperature_2m_min);
                    obj_item.day_max = Math.max(...data.daily.temperature_2m_max);
                    this.collected.push(obj_item);
                })
        }
    },
    methods: {
        sortList(sortBy) {

            let sorted = [];

            if (sortBy === 'city') {
                this.chosenColumn = 'city'
                document.getElementsByClassName('city')[0].classList.add('chosen')
                document.getElementsByClassName('day_min')[0].classList.remove('chosen')
                document.getElementsByClassName('day_max')[0].classList.remove('chosen')
            }
            if (sortBy === 'day_min') {
                this.chosenColumn = 'day_min'
                document.getElementsByClassName('day_min')[0].classList.add('chosen')
                document.getElementsByClassName('city')[0].classList.remove('chosen')
                document.getElementsByClassName('day_max')[0].classList.remove('chosen')
            }
            if (sortBy === 'day_max') {
                this.chosenColumn = 'day_max'
                document.getElementsByClassName('day_max')[0].classList.add('chosen')
                document.getElementsByClassName('city')[0].classList.remove('chosen')
                document.getElementsByClassName('day_min')[0].classList.remove('chosen')
            }

            if (this.sortedbyASC) {
                sorted = this.collected.sort((x, y) => (x[sortBy] > y[sortBy] ? -1 : 1));
                this.sortedbyASC = false;
            } else {
                sorted = this.collected.sort((x, y) => (x[sortBy] < y[sortBy] ? -1 : 1));
                this.sortedbyASC = true;
            }
            this.collected = sorted;
        },
        addCity() {
            this.adding = !this.adding
        },
        showAdd() {
            this.adding = true;
            setTimeout(() => {
                this.adding = false;
            }, 3500)
        },
        addToGeo(event) {

            let addCity = event.target.parentElement.firstElementChild.innerText;

            for (let index in this.add) {

                if (this.add[index].city === addCity) {
                    this.geo.push(this.add[index])
                    this.add.splice(index, 1)
                }

            }
            this.addCity()
            this.reload()
        },
        reload() {
            this.$forceUpdate();
        },
        removeFromGeo(event) {

            let removeCity = event.target.parentElement.firstElementChild.innerText;

            for (let index in this.geo) {

                if (this.geo[index].city === removeCity) {
                    this.add.push(this.geo[index])
                    this.geo.splice(index, 1)
                }

            }
            this.showAdd()
            this.reload()
        },


    },
}
</script>

<style scoped>
#right-click-menu {
    background: #FAFAFA;
    border: 1px solid #BDBDBD;
    box-shadow: 0 2px 2px 0 rgba(0, 0, 0, .14), 0 3px 1px -2px rgba(0, 0, 0, .2), 0 1px 5px 0 rgba(0, 0, 0, .12);
    display: block;
    list-style: none;
    margin: 0;
    padding: 0;
    position: absolute;
    width: 250px;
    z-index: 999999;
}

#right-click-menu li {
    border-bottom: 1px solid #E0E0E0;
    margin: 0;
    padding: 5px 35px;
}

#right-click-menu li:last-child {
    border-bottom: none;
}

#right-click-menu li:hover {
    background: #1E88E5;
    color: #FAFAFA;
}

td {
    height: 4vh;
    width: 33vw;
}

td span,
th:hover,
td:hover {
    background-color: rgb(240, 246, 252);
    color: rgb(22, 27, 34);
    box-shadow: inset 0 0 1vh 1vh rgba(22, 27, 34, 0.66);

}

td span {
    width: 4vh;
    border-radius: 0.25rem;
}

.mirrorVertSpan {
    display: block;
    -moz-transform: scale(1, -1);
    -webkit-transform: scale(1, -1);
    -o-transform: scale(1, -1);
    -ms-transform: scale(1, -1);
    transform: scale(1, -1);
}

.normalVertSpan {
    display: block;
    -moz-transform: scale(1, 1);
    -webkit-transform: scale(1, 1);
    -o-transform: scale(1, 1);
    -ms-transform: scale(1, 1);
    transform: scale(1, 1);
}

.chosen {
    font-size: 0.8rem;
    font-weight: 900;
    letter-spacing: 0.1ch;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
}

.navigation {
    font-weight: 100;
    font-size: 0.75rem;
}

th {
    letter-spacing: 0.1ch;
}

.clickable {
    cursor: pointer;
}

.removeAble {
    cursor: zoom-out;
}

.addAble {
    cursor: zoom-in;
}

table {
    color: rgb(240, 246, 252);
}

th,
td {
    border: 1px solid rgb(43, 49, 56);
    border-radius: 0.5rem;
    background: rgb(22, 27, 34, 0.75);
}

.hovered {
    background-color: rgba(22, 27, 34, 0.5);
}
</style>
