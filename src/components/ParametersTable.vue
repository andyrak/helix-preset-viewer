<template>
    <table class="parameters">
        <tr v-for="[key, value] in modelEntries" :key="`model-${key}`">
            <th>{{ getLabel(key) }}</th><th> {{ getModel(value, 'name') }}</th>
        </tr>
        <tr v-for="[key, value] in parameterEntries" :key="`param-${key}`">
            <td>{{ getLabel(key) }}</td><td> {{ value }} </td>
        </tr>    
    </table>
</template>


<script>
export default {
    name: 'ParametersTable',
    data: function() {
        return {
            preset: this.$root.hxPreset
        }
    },
    props: ['block'],
    computed: {
        modelEntries() {
            if (!this.block) return [];
            return Object.entries(this.block).filter(([key]) => key === '@model');
        },
        parameterEntries() {
            if (!this.block) return [];
            const excludedKeys = ['@model', '@path', 'Voice', '@type', '@bypassvolume', '@cab', '@enabled', 'SSwitch'];
            return Object.entries(this.block).filter(([key]) => !excludedKeys.includes(key));
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
        getLabel(value) {
            if (!value) return '';
            value = value.toString();
            if (typeof this.$root.hxLabels[value] !== "undefined") {
                return this.$root.hxLabels[value];
            } else {
                return value + "*";
            }
        }
    }
}
</script>

<style>
.parameters {
    font-size: 16px;
    background-color: #111;
    margin: 0 auto;
}

.parameters th {
    padding: 15px;
    text-align: left;
    background-color: #222;
}

.parameters td {
    padding: 8px 15px;
    text-align: left;
    background-color: #333;
}
</style>