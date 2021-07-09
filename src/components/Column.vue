<template>
  <div class="culomn" :class="`margin m${num+1}`" :style="getColor(value)">
      <p v-if="value !== 0">{{ value }}</p>
  </div>
</template>

<script>
export default {
    data() {
        return {
            colors: [ 
                '#f0efeb', '#cddafd', '#dfe7fd',  
                '#bee1e6', '#98c1d9', '#fad2e1', 
                '#fde2e4', '#fff1e6', '#eae4e9',
                "#ffd6a5", '#a0c4ff', '#f2cc8f',
                "#ff6b6b", '#118ab2', '#fff3b0'
            ]
        };
    },
    props: ['value', 'num'],
    methods: {
        getColor(value) {
            switch (value) {
                case 0: 
                    return `background-color: ${this.colors[2]};`
                case 2:
                    return `background-color: ${this.colors[1]}; box-shadow: 0 0 2px rgba(0, 0, 0, 0.1);`
                case 4:
                    return `background-color: ${this.colors[3]}; box-shadow: 0 0 2px rgba(0, 0, 0, 0.1);`
                case 8:
                    return `background-color: ${this.colors[4]}; box-shadow: 0 0 2px rgba(0, 0, 0, 0.1);`
                case 16:
                    return `background-color: ${this.colors[5]}; box-shadow: 0 0 2px rgba(0, 0, 0, 0.1);`
                case 32:
                    return `background-color: ${this.colors[6]}; box-shadow: 0 0 2px rgba(0, 0, 0, 0.1);`
                case 64:
                    return `background-color: ${this.colors[7]}; box-shadow: 0 0 2px rgba(0, 0, 0, 0.1);`
                case 128:
                    return `background-color: ${this.colors[8]}; box-shadow: 0 0 2px rgba(0, 0, 0, 0.1);`
                case 256:
                    return `background-color: ${this.colors[9]}; box-shadow: 0 0 2px rgba(0, 0, 0, 0.1);`
                case 512:
                    return `background-color: ${this.colors[10]}; box-shadow: 0 0 2px rgba(0, 0, 0, 0.1);`
                case 1024:
                    return `background-color: ${this.colors[11]}; box-shadow: 0 0 2px rgba(0, 0, 0, 0.1);`
                case 2048:
                    return `background-color: ${this.colors[12]}; box-shadow: 0 0 2px rgba(0, 0, 0, 0.1);`
                case 4096:
                    return `background-color: ${this.colors[13]}; box-shadow: 0 0 2px rgba(0, 0, 0, 0.1);`
                default:
                    return `background-color: ${this.colors[14]}; box-shadow: 0 0 2px rgba(0, 0, 0, 0.1);`   
            }
        }
    },
    created() {
    }
}
</script>

<style scoped>
.culomn {
    background-color: #FFFF0A;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 5px;
}

.margin {
    margin: 2px 0 0 2px;
}

.m4, .m8, .m12, .m16 {
    margin-right: 2px;
}

.m13, .m14, .m15, .m16 {
    margin-bottom: 2px;
}

</style>