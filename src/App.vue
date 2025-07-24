<template>
    <div id="app">
        <div class="container">
            <AppHeader/>
            <div v-if="!presetLoaded" class="path">
                <BlockItem type="input"></BlockItem>
                <BlockItem type="dist"></BlockItem>
                <BlockItem type="comp"></BlockItem>
                <BlockItem type="eq"></BlockItem>
                <BlockItem type="mod"></BlockItem>
                <BlockItem type="delay"></BlockItem>
                <BlockItem type="reverb"></BlockItem>
                <BlockItem type="pitch"></BlockItem>
                <BlockItem type="filter"></BlockItem>
                <BlockItem type="output"></BlockItem>
            </div>
            <div v-if="!presetLoaded" class="path">
                <BlockItem type="split"></BlockItem>
                <BlockItem type="wah"></BlockItem>
                <BlockItem type="amp-cab"></BlockItem>
                <BlockItem type="amp"></BlockItem>
                <BlockItem type="none"></BlockItem>
                <BlockItem type="preamp"></BlockItem>
                <BlockItem type="cab"></BlockItem>
                <BlockItem type="ir"></BlockItem>
                <BlockItem type="vol-pan"></BlockItem>
                <BlockItem type="send-return"></BlockItem>
                <BlockItem type="looper"></BlockItem>
                <BlockItem type="join"></BlockItem>
            </div>
            
            <h2 v-if="presetLoaded">Preset Name: {{ preset.data.meta.name }}</h2>
            
            <div v-if="presetLoaded" class="path">
                <BlockItem     
                    v-for="[key, value] in dsp0InputA" 
                    :key="`dsp0-${key}`"
                    v-bind:type="getModel(value['@model'], 'type')" v-bind:id="value" v-bind:position="0">
                </BlockItem>
                <BlockItem     
                    v-for="[key, value] in dsp0SplitJoin" 
                    :key="`dsp0-${key}`"
                    v-bind:type="getModel(value['@model'], 'type')" v-bind:id="value" v-bind:position="value['@position']">
                </BlockItem>
                <BlockItem     
                    v-for="[key, value] in dsp0Path0Blocks" 
                    :key="`dsp0-${key}`"
                    v-bind:type="getModel(value['@model'], 'type')" v-bind:id="value" v-bind:position="value['@position']">
                </BlockItem>
                <BlockItem     
                    v-for="[key, value] in dsp0OutputA" 
                    :key="`dsp0-${key}`"
                    v-bind:type="getModel(value['@model'], 'type')" v-bind:id="value" v-bind:position="20">
                </BlockItem>
            </div>
            <div v-if="presetLoaded" class="path">
                <BlockItem     
                    v-for="[key, value] in dsp0InputB" 
                    :key="`dsp0-${key}`"
                    v-bind:type="getModel(value['@model'], 'type')" v-bind:id="value" v-bind:position="0">
                </BlockItem>
                <BlockItem     
                    v-for="[key, value] in dsp0Path1Blocks" 
                    :key="`dsp0-${key}`"
                    v-bind:type="getModel(value['@model'], 'type')" v-bind:id="value" v-bind:position="value['@position']">
                </BlockItem>
                <BlockItem     
                    v-for="[key, value] in dsp0OutputB" 
                    :key="`dsp0-${key}`"
                    v-bind:type="getModel(value['@model'], 'type')" v-bind:id="value" v-bind:position="20">
                </BlockItem>
            </div>
            <div v-if="presetLoaded" class="path">
                <BlockItem     
                    v-for="[key, value] in dsp1InputA" 
                    :key="`dsp1-${key}`"
                    v-bind:type="getModel(value['@model'], 'type')" v-bind:id="value" v-bind:position="0">
                </BlockItem>
                <BlockItem     
                    v-for="[key, value] in dsp1SplitJoin" 
                    :key="`dsp1-${key}`"
                    v-bind:type="getModel(value['@model'], 'type')" v-bind:id="value" v-bind:position="value['@position']">
                </BlockItem>
                <BlockItem     
                    v-for="[key, value] in dsp1Path0Blocks" 
                    :key="`dsp1-${key}`"
                    v-bind:type="getModel(value['@model'], 'type')" v-bind:id="value" v-bind:position="value['@position']">
                </BlockItem>
                <BlockItem     
                    v-for="[key, value] in dsp1OutputA" 
                    :key="`dsp1-${key}`"
                    v-bind:type="getModel(value['@model'], 'type')" v-bind:id="value" v-bind:position="20">
                </BlockItem>
            </div>
            <div v-if="presetLoaded" class="path">
                <BlockItem     
                    v-for="[key, value] in dsp1InputB" 
                    :key="`dsp1-${key}`"
                    v-bind:type="getModel(value['@model'], 'type')" v-bind:id="value" v-bind:position="0">
                </BlockItem>
                <BlockItem     
                    v-for="[key, value] in dsp1Path1Blocks" 
                    :key="`dsp1-${key}`"
                    v-bind:type="getModel(value['@model'], 'type')" v-bind:id="value" v-bind:position="value['@position']">
                </BlockItem>
                <BlockItem     
                    v-for="[key, value] in dsp1OutputB" 
                    :key="`dsp1-${key}`"
                    v-bind:type="getModel(value['@model'], 'type')" v-bind:id="value" v-bind:position="20">
                </BlockItem>
            </div>
            
            <ParametersTable v-bind:block="currentBlock"></ParametersTable>
            
            <AppFooter/>
        </div>
    </div>
</template>

<script>

import AppHeader from './components/Header.vue'
import AppFooter from './components/Footer.vue'
import BlockItem from './components/BlockItem.vue'
import ParametersTable from './components/ParametersTable.vue'

export default {
    name: 'app',
    components: {
        AppHeader,
        AppFooter,
        BlockItem,
        ParametersTable
    },
    data: function() {
        return {
            preset: this.$root.hxPreset,
            models: this.$root.hxModels,
            currentBlock: {},
            presetLoaded: false
        }
    },
    computed: {
        dsp0InputA() {
            if (!this.presetLoaded || !this.preset.data.tone.dsp0) return [];
            return Object.entries(this.preset.data.tone.dsp0).filter(([key]) => key === 'inputA');
        },
        dsp0SplitJoin() {
            if (!this.presetLoaded || !this.preset.data.tone.dsp0) return [];
            return Object.entries(this.preset.data.tone.dsp0).filter(([key]) => key === 'split' || key === 'join');
        },
        dsp0Path0Blocks() {
            if (!this.presetLoaded || !this.preset.data.tone.dsp0) return [];
            return Object.entries(this.preset.data.tone.dsp0).filter(([key, value]) => value['@path'] === 0 && key.startsWith('block'));
        },
        dsp0OutputA() {
            if (!this.presetLoaded || !this.preset.data.tone.dsp0) return [];
            return Object.entries(this.preset.data.tone.dsp0).filter(([key]) => key === 'outputA');
        },
        dsp0InputB() {
            if (!this.presetLoaded || !this.preset.data.tone.dsp0) return [];
            return Object.entries(this.preset.data.tone.dsp0).filter(([key]) => key === 'inputB');
        },
        dsp0Path1Blocks() {
            if (!this.presetLoaded || !this.preset.data.tone.dsp0) return [];
            return Object.entries(this.preset.data.tone.dsp0).filter(([key, value]) => value['@path'] === 1 && key.startsWith('block'));
        },
        dsp0OutputB() {
            if (!this.presetLoaded || !this.preset.data.tone.dsp0) return [];
            return Object.entries(this.preset.data.tone.dsp0).filter(([key]) => key === 'outputB');
        },
        dsp1InputA() {
            if (!this.presetLoaded || !this.preset.data.tone.dsp1) return [];
            return Object.entries(this.preset.data.tone.dsp1).filter(([key]) => key === 'inputA');
        },
        dsp1SplitJoin() {
            if (!this.presetLoaded || !this.preset.data.tone.dsp1) return [];
            return Object.entries(this.preset.data.tone.dsp1).filter(([key]) => key === 'split' || key === 'join');
        },
        dsp1Path0Blocks() {
            if (!this.presetLoaded || !this.preset.data.tone.dsp1) return [];
            return Object.entries(this.preset.data.tone.dsp1).filter(([key, value]) => value['@path'] === 0 && key.startsWith('block'));
        },
        dsp1OutputA() {
            if (!this.presetLoaded || !this.preset.data.tone.dsp1) return [];
            return Object.entries(this.preset.data.tone.dsp1).filter(([key]) => key === 'outputA');
        },
        dsp1InputB() {
            if (!this.presetLoaded || !this.preset.data.tone.dsp1) return [];
            return Object.entries(this.preset.data.tone.dsp1).filter(([key]) => key === 'inputB');
        },
        dsp1Path1Blocks() {
            if (!this.presetLoaded || !this.preset.data.tone.dsp1) return [];
            return Object.entries(this.preset.data.tone.dsp1).filter(([key, value]) => value['@path'] === 1 && key.startsWith('block'));
        },
        dsp1OutputB() {
            if (!this.presetLoaded || !this.preset.data.tone.dsp1) return [];
            return Object.entries(this.preset.data.tone.dsp1).filter(([key]) => key === 'outputB');
        }
    },
    methods: {
        getModel(value, prop) {
            if (!value) return '';
            value = value.toString();
            if (typeof this.$root.hxModels[value] !== "undefined") {
                return this.$root.hxModels[value][prop];
            } else {
                return "unknown";
            }
        },
        /* file upload based on help from https://codepen.io/Atinux/pen/qOvawK & https://stackoverflow.com/questions/14740927/using-html-5-file-api-to-load-a-json-file */
        onFileChange(e) { 
            var files = e.target.files || e.dataTransfer.files;
                if (!files.length)
                    return;
                this.loadPreset(files[0]);
        },
        loadPreset(file) {
              var preset = null;
              var reader = new FileReader();
              var vm = this;

              reader.onload = (e) => {
                preset = JSON.parse(e.target.result);
                vm.presetLoaded = true;
                vm.preset = preset;
              };
              reader.readAsText(file);
        }
    }
}

</script>

<style>
body {
    color: #ccc;
    background-color: #000;
    font-family: Helvetica, Arial, sans-serif;
}

a:link {
    color: #ccc;
}

a:visited {
    color: #ccc;
}

a:hover {
    color: #fff;
}

a:active {
    color: #fff;
}

li {
    margin-bottom: 5px;
}

.container {
    width: 80%;
    margin: 0 auto;
}

.header {
    width: 80%;
    margin: 50px auto;
}

.footer {
    font-size: 14px;
    text-align: center;
    margin: 50px auto;
}

.path {
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
    margin-bottom: 20px;
    min-height: 70px;
}

.path::before {
    content: " ";
    width: 100%;
    position: absolute;
    height: 1px;
    background-color: #ccc;
    top: 40%;
}
</style>
