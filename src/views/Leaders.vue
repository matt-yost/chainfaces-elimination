<template>
    <div class="home">
        <h1>Leaders</h1>
        <table class="table">
            <thead>
            <tr>
                <th>Rank</th>
                <th scope="col">Owner</th>
                <th scope="col">ENS</th>
                <th scope="col">Total</th>
                <th scope="col">Entered</th>
                <th scope="col">Withdrawn</th>
                <th scope="col">Dead</th>
                <th scope="col">Alive</th>
                <th scope="col">% Dead</th>
                <th scope="col">% of Pool</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="v, i in getLeaderList" :key="v.owner">
                <td>{{ i + 1 }}</td>
                <td>
                    <a :href="getLink(v)" target="_blank">{{ v.owner }}</a>
                </td>
                <td>
                    {{ens[v.owner]}}
                </td>
                <td>{{ v.totalTokens }}</td>
                <td>{{ v.totalEntered }}</td>
                <td>{{ v.totalCowards }}</td>
                <td>{{ v.totalDeaths }}</td>
                <td>{{ v.totalAlive }}</td>
                <td>{{ v.percentageDead }}</td>
                <td>{{ v.percentageOfAlive }}</td>
            </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import {mapGetters} from "vuex";


export default {
    name: 'Home',
    data() {
        return {
            ens : {},
            list: [],
        }
    },
    methods: {
        getLink(v) {
            return 'https://chainfacesarena.com/#/arena/'+v.owner;
        }
    },
    computed: {
        ...mapGetters([
            'getLeaderList',
            'getEns',
            'totalTokensByAddress'
        ])
    },
    async mounted() {
        await this.$store.dispatch('getWeb3');

        let ens = this.getEns;

        // this.list = await this.$store.dispatch('loadLeaderList');
        let tokens = this.getLeaderList;

        for (const i of tokens) {
            let ensName = null;
            ({ name: ensName } = await ens.getName(i.owner))
            console.log(ensName, i.owner);
            if (ensName) {
                this.ens[i.owner] = ensName;
            }
        }
        this.$forceUpdate();
    }
}
</script>
