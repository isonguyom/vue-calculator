<script>
import Display from './Display.vue';
import ButtonWrapper from './ButtonWrapper.vue';
export default {
    components: { Display, ButtonWrapper },

    data() {
        return {
            entries: "",
            entriesLength: false,
            isPositive: true,
            ans: "0",
            mAns: "",
            result: 0,
            hasDot: false,
            isError: false
        }
    },

    methods: {
        addNum(inp) {
            this.isError = false
            this.entries += inp
        },

        addOps(op) {
            this.isError = false
            let ops = ["+", "-", "/", "*", "%", "√", "^"]
            if (this.entries != "" && this.entries != "√" && this.entries != "-" && this.entries != "Bad Expression") {
                if (ops.includes(this.entries.charAt(this.entries.length - 1))) {
                    if (op == "-") {
                        if (this.entries.charAt(this.entries.length - 1) == "+" || this.entries.charAt(this.entries.length - 1) == "-") {
                            this.replaceChar(op, this.entries.length - 1)
                        } else if (this.entries.charAt(this.entries.length - 1) == "√") {
                            this.entries = this.entries
                        }
                        else {
                            this.entries += op
                        }
                    } if (ops.includes(this.entries.charAt(this.entries.length - 2))) {
                        if (this.entries.charAt(this.entries.length - 1) == "√") {
                            if (op == "√") {
                                this.entries += op
                            } else {
                                this.entries = this.entries
                            }
                        } else if (this.entries.charAt(this.entries.length - 1) == "-") {
                            if (op == "√") {
                                this.replaceChar(op, this.entries.length - 1)
                            } else {
                                this.entries = this.entries
                            }
                        } else {
                            this.entries = this.entries
                        }
                    } else if (op == "√") {
                        this.entries += op
                    } else {
                        this.replaceChar(op, this.entries.length - 1)
                    }
                } else {
                    this.entries += op
                }
            } else if (op == "-" || op == "√") {
                this.entries += op
            } else {

                this.entries = this.entries
            }
            this.hasDot = false
        },

        operations(ops) {
            this.isError = false
            if (ops == "C") {
                this.entries = ""
            }
            if (ops == "DEL") {
                this.replaceChar("", this.entries.length - 1)

            }
            if (ops == "ANS") {
                this.entries = this.ans
            }
            if (ops == "=") {
                this.calculate(this.entries)
            }
            if (ops == "M+") {
                this.ans++
                this.entries = this.ans.toString()

            }
            if (ops == "M-") {
                this.ans--
                this.entries = this.ans.toString()
            }
        },

        addSign(sign) {
            this.isError = false
            if (sign == ".") {
                if (!this.hasDot) {
                    this.entries += sign
                    this.hasDot = true
                } else {
                    this.entries = this.entries
                }
            }
        },

        replaceChar(newChar, index) {
            this.entries = this.entries.split("");

            this.entries[index] = newChar;

            this.entries = this.entries.join("");

            return this.entries;
        },

        calculate(s) {
            if (!this.entries == "") {


                for (let i = 0; i < s.length; i++) {
                    if (s[i] == "^") {
                        s = s.split("");

                        s[i] = "**";

                        s = s.join("");
                    }

                    if (s[i] == "√") {
                        s = s.split("");

                        let next = s[i + 1]
                        let newVal = "0.5**" + next.toString()
                        if (isNaN(parseFloat(s[i - 1]))) {
                            s.splice(i, 2, newVal)
                        } else {
                            s.splice(i, 2, "*", newVal)
                        }

                        s = s.join("");
                    }
                }

                console.log(s)
                try {
                    s = eval(s)
                    this.ans = s.toString()
                    this.entries = s.toString()
                    // this.isError = false
                }
                catch (err) {
                    this.isError = true
                    this.entries = "Bad Expression";
                }
            }
        },

        // entriesLen(str) {
        //     if (str.length > 12) {
        //         str = Math.exp(eval(str))
        //     console.log(str + "is greater than 10")
        //     }
        //     this.entries = str.toString()

        //     // if (str.length > 13) {

        //     //     str = str.split("");


        //     //     s = s.join("");
        //     // }
        //     // this.entries = str
        //     return this.entries
        // }
    },

    // mounted() {
    //     this.entriesLen(this.entries)
    // }
}
</script>

<template>
    <section class="cal-body">
        <Display :entries="entries" :isError="isError"></Display>
        <ButtonWrapper @add-num="addNum" @add-ops="addOps" @add-sign="addSign" @add-cal="operations"></ButtonWrapper>
    </section>
</template>

<style scoped>
.cal-body {
    width: 350px;
    height: 500px;
    margin-left: auto;
    margin-right: auto;
    color: #fff;
    background-color: rgba(255, 255, 255, 0.06);
    border: 1px solid rgba(255, 255, 255, 0.1);
    border-radius: 15px;
    backdrop-filter: blur(10px);
}
</style>