<template>
    <div class="number-pad">
        <div class="amount">{{amount}}</div>
        <div class="number">
            <button @click="enterNumber">1</button>
            <button @click="enterNumber">2</button>
            <button @click="enterNumber">3</button>
            <button @click="del">删除</button>
            <button @click="enterNumber">4</button>
            <button @click="enterNumber">5</button>
            <button @click="enterNumber">6</button>
            <button @click="clear">清空</button>
            <button @click="enterNumber">7</button>
            <button>8</button>
            <button @click="enterNumber">9</button>
            <button @click="confirm" class="ok-button">OK</button>
            <button @click="enterNumber" class="zero-button">0</button>
            <button @click="enterNumber">.</button>
        </div>
    </div>
</template>

<script lang="ts">
  import Vue from "vue"
  import {Component, Prop} from "vue-property-decorator"

  @Component
  export default class NumberPad extends Vue {
    @Prop(Number) readonly output!: number
    amount = this.output.toString()

    del() {
      this.amount = this.amount.slice(0, -1) || '0'
    }

    clear() {
      this.amount = '0'
    }

    confirm() {
      this.$emit('update:output', parseFloat(this.amount))
      this.$emit('submit', this.amount)
      this.amount = '0'
    }

    enterNumber(event: MouseEvent) {
      const button = (event.target as HTMLButtonElement)
      const text = button.textContent as string

      if (this.amount.length === 16) return
      if (this.amount.indexOf('.') >= 0 && text === '.') return

      if (this.amount === "0") {
        if ("0123456789".indexOf(text) >= 0) {
          this.amount = text
        } else {
          this.amount += text
        }
      }
      else {
        this.amount += text
      }
    }
  }
</script>

<style scoped lang="scss">
    @import "~@/assets/styles/helper.scss";

    .number-pad {
        .amount {
            @extend %clearfix;
            @extend %innerShadow;
            text-align: right;
            font-size: 36px;
            font-family: Consolas, monospace;
            padding: 9px 16px;
            height: 70px;
        }

        .number {
            @extend %clearfix;

            > button {
                width: 25%;
                height: 64px;
                float: left;
                background: transparent;
                border: none;

                &.ok-button {
                    height: 64*2px;
                    float: right;
                }

                &.zero-button {
                    width: 25*2%;
                }

                $bg: #f2f2f2;

                &:nth-child(1) {
                    background: $bg;
                }

                &:nth-child(2), &:nth-child(5) {
                    background: darken($bg, 4%);
                }

                &:nth-child(3), &:nth-child(6), &:nth-child(9) {
                    background: darken($bg, 4*2%);
                }

                &:nth-child(4), &:nth-child(7), &:nth-child(10) {
                    background: darken($bg, 4*3%);
                }

                &:nth-child(8), &:nth-child(11), &:nth-child(13) {
                    background: darken($bg, 4*4%);
                }

                &:nth-child(14) {
                    background: darken($bg, 4*5%);
                }

                &:nth-child(12) {
                    background: darken($bg, 4*6%);
                }
            }
        }
    }
</style>
