<template>
  <div class="is-editor-wrapper">
       <div class="buttonsWrapper">
            <transition name="fade" mode="out-in">
                <span v-if="!reverse" :key="1">
                    <button
                        @click="reverseElements"
                        class="button is-primary mt-5 pl-4"
                    >
                        Reverse
                        <i class="fas fa-eye pl-1 pr-2" /> 
                    </button>
                </span>
                <span v-else :key="2">
                    <button
                        @click="reverseElements"
                        class="button is-primary mt-5 pl-4 is-center"
                    >
                        Reverse back
                        <i class="fas fa-highlighter pl-1 pr-2" /> 
                    </button>
                </span>
            </transition>
        </div>
        <br>
        <transition name="fade" mode="out-in">
            <div v-if="!reverse" :key="1">
                <textarea
                    v-bind:value="inputText"
                    @input="updateInputText($event.target.value)"
                    class="is-editor pt-5 pb-5 is-primary mt-5"
                    placeholder="Type your text here"
                />
                <p
                    class="is-result pt-5 pb-5 is-primary mt-3"
                    placeholder="Type your binary here"
                >
                    {{binaryValue}}
                </p>
            </div>
            <div v-else>
                <textarea
                    v-bind:value="inputBinary"
                    @input="updateInputBinary($event.target.value)"
                    class="is-editor pt-5 pb-5 is-primary mt-5"
                    placeholder="Type your Binary here"
                    type="number"
                />
                <p
                    class="is-result pt-5 pb-5 is-primary mt-3 is-breakable"
                    placeholder="Type your binary here"
                >
                    {{textValue}}
                </p>
            </div>
        </transition>
  </div>
</template>

<script>
export default {
    name: 'Editor',
    components: {},
    props: {
        inputFile: {
            type: String,
            required: false,
            default: ''
        },
        share: {
            type: Boolean,
            required: true
        }
    },
    data () {
        return  {
            inputText: '',
            inputBinary: 0,
            reverse: false,
            markdownWrapper: '',
            saveFileModalOpen: false
        }
    },
    watch: {
        binaryValue (val) {
            this.inputBinary = val
        },
        textValue (val) {
            this.inputText = val
        }
    },
    computed: {
        binaryValue () {
            let value = ''
            for (let i=0; i < this.inputText.length; i++) {
                value += this.inputText[i].charCodeAt(0).toString(2) + ' '
            }
            return value
        },
        textValue () {
            let newBin = ''
            newBin = this.inputBinary.split(" ");
            let binCode = [];

            for (let i = 0; i < newBin.length; i++) {
                binCode.push(String.fromCharCode(parseInt(newBin[i], 2)));
            }
            return binCode.join("");
        }
    },
    methods: {
        reverseElements () {
            this.reverse = !this.reverse
            if (this.reverse) {
                this.inputBinary = this.binaryValue
                return
            }
            if (!this.inputBinary.length) {
                this.inputText = ''
                return    
            }
            this.inputText = this.textValue
        },
        updateInputText (text) {
            this.inputText = text
        },
        updateInputBinary (binary) {
            if (binary !== '0' && binary !== '1')
            this.inputBinary = binary
        }
    }
}
</script>