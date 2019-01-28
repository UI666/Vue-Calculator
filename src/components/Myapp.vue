<template>
    <main class = "calculator">

        <div class="display">
            <h1>{{displayHeder}}</h1>
            <p>{{displayData}}</p>
        </div>


        <div class = "container">
            <div class = "tax">
                <h1>{{taxHeader}}</h1>
                <p>{{taxNumber}}{{taxPercentage}}</p>
            </div>
            <div class = "input">
                <h1>{{inputHeder}}</h1>
                <p>{{inputData}}</p>
            </div>
            <div class = "button-holder">
                <button class = "clear-button" v-on:click="clearInput">x</button>
            </div>
        </div>


        <div class = "buttons">
            <div class ="button-row" v-for="row in buttonRows">
                <div class = "button" v-for="button in row"
                    v-bind:class = "{operator: button.type == 'operator'}"
                    @click = "buttonClick(button)">
                    <p>{{button.text}}</p>
                </div>
            </div>
        </div>
    </main>
</template>


<script>
    export default
    {
        methods: {
            buttonClick(button){
                if(button.type == 'number' && this.inputData.length < 8) {
                    if (button.text == '.' && this.inputData.includes('.')) return;
                    this.inputData += button.text;
                    if(this.inputData.length > 1 && this.inputData[0] == '0' && this.inputData[1] != '.') {
                      this.inputData = this.inputData.slice(1);
                    }
                    this.displayData = '0';
                }
                else if(button.type == 'operator'){
                    this.displayData = Number((Number(this.inputData) + ((Number(this.taxNumber)*Number(this.inputData))/100)).toFixed(2));
                    this.inputData = '0';
                }
            },
            clearInput(){
                this.inputData = '0';
            }
        },

        data: () => ({
            displayHeder: '----------   Final Price   ----------',
            displayData: '0',
            taxHeader: 'tax',
            taxNumber: '8',
            taxPercentage: '%',
            inputHeder: 'Before Tax',
            inputData: '0',

            buttonRows: [
              [{text: '7', type: 'number'}, {text: '8', type: 'number'}, {text: '9', type: 'number'}],
              [{text: '4', type: 'number'},{text: '5', type: 'number'}, {text: '6', type: 'number'}],
              [{text: '1', type: 'number'},{text: '2', type: 'number'},{text: '3', type: 'number'}],
              [{text: '.', type: 'number'},{text: '0', type: 'number'},{text: 'x', type: 'operator'}]
            ]
        })
    };
</script>
