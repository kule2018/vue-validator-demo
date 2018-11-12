<template>
<div>
    <div class="form" v-if="show">
        <h3>表单</h3>
        <div class="item">
            <span class="item-left">原生HTML INPUT标签:</span>

            <div class="item-right">
                <input placeholder="请输入"
                       :class="['native-input', { 'error': !errors.get('nativeInput').success }]"
                       v-model="form.nativeInput"
                       v-validate:nativeInput.blur="validates.nativeInput"
                       @focus="errors.reset('nativeInput')"
                />

                <span class="u-info">必填</span>
                <span class="u-info">{{ errors.get('nativeInput').warn }}</span>
            </div>
        </div>

        <div class="item">
            <span class="item-left">Element INPUT组件:</span>

            <div class="item-right">
                <el-input
                    placeholder="请输入内容"
                    :class="[{ 'error': !errors.get('elmInput').success }]"
                    v-model="form.elmInput"
                    v-validate:elmInput.blur="validates.elmInput"
                    @focus="errors.reset('elmInput')"
                />

                <span class="u-info">必填</span>
                <span class="u-info">{{ errors.get('elmInput').warn }}</span>
            </div>
        </div>

        <div class="item">
            <span class="item-left">Element Textarea组件:</span>

            <div class="item-right">
                <el-input
                    type="textarea"
                    :rows="2"
                    placeholder="请输入内容"
                    v-model="form.textarea">
                </el-input>
            </div>
        </div>

        <div class="item">
            <span class="item-left">Element Checkbox组件:</span>

            <div class="item-right">
                <el-checkbox-group
                    :class="[{ 'error': !errors.get('checkbox').success }]"
                    v-model="form.checkbox"
                    v-validate:checkbox.blur="validates.checkbox"
                >
                    <el-checkbox label="复选框 A"></el-checkbox>
                    <el-checkbox label="复选框 B"></el-checkbox>
                    <el-checkbox label="复选框 C"></el-checkbox>
                </el-checkbox-group>

                <span class="u-info">必填</span>
                <span class="u-info">{{ errors.get('checkbox').warn }}</span>
            </div>
        </div>

        <div class="item">
            <span class="item-left">Element Select组件:</span>

            <div class="item-right">
                <el-select
                    placeholder="请选择"
                    :class="[{ 'error': !errors.get('select').success }]"
                    v-model="form.select"
                    v-validate:select="validates.select"
                >
                    <el-option
                        v-for="item in options"
                        :key="item.value"
                        :label="item.label"
                        :value="item.value"
                    >
                    </el-option>
                </el-select>

                <span class="u-info">必填</span>
                <span class="u-info">{{ errors.get('select').warn }}</span>
            </div>
        </div>

        <div class="item btns">
            <el-button type="primary" @click.stop="submit">提交</el-button>
            <el-button type="primary" @click.stop="reset">重置</el-button>
        </div>
    </div>
</div>
</template>

<script>
import { validateResult } from 'fat-validator'

/* eslint-disable */
export default {
    mixins: [ validateResult ],

    props: {
        options: { type: Array, default: () => [
            {
                value: '选项1',
                label: '1'
            }, {
                value: '选项2',
                label: '2'
            }, {
                value: '选项3',
                label: '3'
            }]
        }
    },

    data () {
        return {
            form: {
                nativeInput: '',
                elmInput: '',
                textarea: '',
                checkbox: ['复选框 A'],
                select: ''
            },

            validates: {
                nativeInput: [
                    {
                        need: () => !!this.form.nativeInput,
                        warn: '不能为空'
                    }
                ],
                elmInput: [
                    {
                        need: () => !!this.form.elmInput,
                        warn: '不能为空'
                    }
                ],
                checkbox: [
                    {
                        need: () => {
                            return this.form.checkbox.length !== 0
                        },
                        warn: '请选择'
                    }
                ],
                select: [
                    {
                        need: () => !!this.form.select,
                        warn: '请选择'
                    }        
                ]
            },

            show: true
        }
    },

    methods: {
        submit () {
            console.log(this.errors.validateAll())
        },
        reset () {
            console.log(this.errors.resetAll())
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.form {    
    padding: 32px;
    width: 600px;

    box-shadow: 0 0 10px #bbb;
}
.form .item {
    display: flex;
    justify-content: flex-start;
    align-items: center;
}
.form .item .item-left {
    margin-right: 12px;
    width: 185px;
    text-align: left;
}
.form .item .item-right {
    position: relative;
    text-align: left;
}
.form .item:not(:last-child) {
    margin-bottom: 40px;
}
.item .u-info {
    position: absolute;
    top: 100%;

    font-size: 0.8em;
    color: #bbb;
}

.u-info + .u-info {
    margin-top: 1em;
    color: #ff0000;
}

.form .btns {
    margin-top: 16px;
    justify-content: center;
}
.item .native-input {
    height: 38px;
}
</style>
