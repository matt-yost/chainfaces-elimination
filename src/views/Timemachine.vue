<template>
    <div class="home">
        <h1>CHAINFACE TIME MACHINE</h1>
        <div class="row">
            <div class="col-1">Token ID</div>
            <div class="col-2"><input type="text" class="input" v-model="tokenId" /></div>
            <div class="col-2"><button class="btn btn-primary" @click="fetch">TIME-MACHINE</button></div>
        </div>
        <div class="row">
            <div class="col-6">
                <img src="@/assets/TimeMachine.gif" class="img" v-if="loading" />
                <div v-html="svg" v-if="toggle"></div>
                <div v-if="toggle">Rounds Survived: {{rounds}}</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            tokenId: null,
            svg: null,
            contract: null,
            toggle: false,
            loading: false,
            rounds: null,
        }
    },
    methods: {
        async fetch() {
            this.loading = true;
            this.toggle = false;

            setTimeout( () => {
                this.loading = false;
                this.toggle = true;
            }, 10000);

            this.svg = await this.$store.dispatch("getSvg", this.tokenId);
            this.rounds = await this.$store.dispatch("roundsSurvived", this.tokenId);
        }
    },
    async mounted() {
        this.contract = await this.$store.dispatch('getWeb3');
    }
}
</script>