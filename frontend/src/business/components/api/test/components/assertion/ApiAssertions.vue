<template>
  <div>
    <div class="assertion-add">
      <el-row :gutter="10">
        <el-col :span="4">
          <el-select :disabled="isReadOnly" class="assertion-item" v-model="type" :placeholder="$t('api_test.request.assertions.select_type')"
                     size="small">
            <el-option :label="$t('api_test.request.assertions.text')" :value="options.TEXT"/>
            <el-option :label="$t('api_test.request.assertions.regex')" :value="options.REGEX"/>
            <el-option :label="'JSONPath'" :value="options.JSON_PATH"/>
            <el-option :label="'JSR223'" :value="options.JSR223"/>
            <el-option :label="$t('api_test.request.assertions.response_time')" :value="options.DURATION"/>
          </el-select>
        </el-col>
        <el-col :span="20">
          <ms-api-assertion-text :is-read-only="isReadOnly" :list="assertions.regex" v-if="type === options.TEXT" :callback="after"/>
          <ms-api-assertion-regex :is-read-only="isReadOnly" :list="assertions.regex" v-if="type === options.REGEX" :callback="after"/>
          <ms-api-assertion-json-path :is-read-only="isReadOnly" :list="assertions.jsonPath" v-if="type === options.JSON_PATH" :callback="after"/>
          <ms-api-assertion-duration :is-read-only="isReadOnly" v-model="time" :duration="assertions.duration"
                                     v-if="type === options.DURATION" :callback="after"/>
          <ms-api-assertion-jsr223  :is-read-only="isReadOnly" :list="assertions.jsr223" v-if="type === options.JSR223" :callback="after"/>
          <el-button v-if="!type" :disabled="true" type="primary" size="small">Add</el-button>
        </el-col>
      </el-row>
    </div>

    <ms-api-assertions-edit :is-read-only="isReadOnly" :assertions="assertions"/>
  </div>
</template>

<script>
  import MsApiAssertionText from "./ApiAssertionText";
  import MsApiAssertionRegex from "./ApiAssertionRegex";
  import MsApiAssertionDuration from "./ApiAssertionDuration";
  import {ASSERTION_TYPE, Assertions} from "../../model/ScenarioModel";
  import MsApiAssertionsEdit from "./ApiAssertionsEdit";
  import MsApiAssertionJsonPath from "./ApiAssertionJsonPath";
  import MsApiAssertionJsr223 from "./ApiAssertionJsr223"

  export default {
    name: "MsApiAssertions",

    components: {
      MsApiAssertionJsr223,
      MsApiAssertionJsonPath,
      MsApiAssertionsEdit, MsApiAssertionDuration, MsApiAssertionRegex, MsApiAssertionText},

    props: {
      assertions: Assertions,
      isReadOnly: {
        type: Boolean,
        default: false
      }
    },

    data() {
      return {
        options: ASSERTION_TYPE,
        time: "",
        type: "",
      }
    },

    methods: {
      after() {
        this.type = "";
      }
    }

  }
</script>

<style scoped>
  .assertion-item {
    width: 100%;
  }

  .assertion-add {
    padding: 10px;
    border: #DCDFE6 solid 1px;
    margin: 5px 0;
    border-radius: 5px;
  }
</style>
