<script setup lang="ts">
import { ProjectConfElement } from "../../../project-conf/projectConfiguration";
import { useProjectConfStore } from "../store";
import ArrayElement from "./ArrayElement.vue";
import DictionaryElement from "./DictionaryElement.vue";
import SelectElement from "./SelectElement.vue";
import StringElement from "./StringElement.vue";

const store = useProjectConfStore();

const props = defineProps<{
  el: ProjectConfElement;
  title: string;
}>();

const openOcdDebugLevelOptions: { name: string; value: number }[] = [
  { name: "Error", value: 0 },
  { name: "Warning", value: 1 },
  { name: "Info", value: 2 },
  { name: "Debug", value: 3 },
  { name: "Verbose", value: 4 },
];

function updateElement(sections: string[], newValue: any) {
  store.updateConfigElement({ confKey: props.title, sections, newValue });
}

function openBuildDir(sections: string[]) {
  store.openBuildPath({ confKey: props.title, sections });
}

function openFilePath(sections: string[]) {
  store.openFilePath({ confKey: props.title, sections });
}

function addValueToArray(sections: string[], newValue: any) {
  store.addValueToConfigElement({
    confKey: props.title,
    sections,
    valueToAdd: newValue,
  });
}

function removeValueFromArray(sections: string[], index: number) {
  store.removeValueFromConfigElement({ confKey: props.title, sections, index });
}
</script>

<template>
  <div class="notification">
    <h2 class="title centerize">{{ title }}</h2>
    <a class="delete" @click="$emit('delete')"></a>
    <label class="is-size-4 has-text-weight-bold">Build</label>
    <div class="small-margin">
      <ArrayElement
        title="Compile arguments"
        :values="el.build.compileArgs ? el.build.compileArgs : []"
        :sections="['build', 'compileArgs']"
        :addValue="addValueToArray"
        :removeValue="removeValueFromArray"
      />
      <ArrayElement
        title="Ninja arguments"
        :values="el.build.ninjaArgs ? el.build.ninjaArgs : []"
        :sections="['build', 'ninjaArgs']"
        :addValue="addValueToArray"
        :removeValue="removeValueFromArray"
      />
      <StringElement
        title="Build Directory path"
        :value="el.build.buildDirectoryPath ? el.build.buildDirectoryPath : ''"
        :sections="['build', 'buildDirectoryPath']"
        :updateMethod="updateElement"
        :openMethod="openBuildDir"
      />
      <ArrayElement
        title="sdkconfig defaults"
        :values="el.build.sdkconfigDefaults ? el.build.sdkconfigDefaults : []"
        :sections="['build', 'sdkconfigDefaults']"
        :addValue="addValueToArray"
        :removeValue="removeValueFromArray"
      />
      <StringElement
        title="sdkconfig file path"
        :value.sync="
          el.build.sdkconfigFilePath ? el.build.sdkconfigFilePath : ''
        "
        :sections="['build', 'sdkconfigFilePath']"
        :updateMethod="updateElement"
        :openMethod="openFilePath"
      />
    </div>
    <DictionaryElement
      title="Environment variables"
      :elements="el.env"
      :sections="['env']"
      :updateMethod="updateElement"
    />
    <StringElement
      title="Flash baud rate"
      :value="el.flashBaudRate ? el.flashBaudRate : ''"
      :sections="['flashBaudRate']"
      :updateMethod="updateElement"
    />
    <StringElement
      title="Monitor baud rate"
      :value="el.monitorBaudRate ? el.monitorBaudRate : ''"
      :sections="['monitorBaudRate']"
      :updateMethod="updateElement"
    />

    <label class="is-size-4 has-text-weight-bold">OpenOCD</label>
    <div class="small-margin">
      <SelectElement
        :selectValue="el.openOCD.debugLevel ? el.openOCD.debugLevel : ''"
        title="Debug Level"
        :options="openOcdDebugLevelOptions"
        :sections="['openOCD', 'debugLevel']"
        :updateMethod="updateElement"
      />
      <ArrayElement
        title="Config files"
        :values="el.openOCD.configs ? el.openOCD.configs : []"
        :sections="['openOCD', 'configs']"
        :addValue="addValueToArray"
        :removeValue="removeValueFromArray"
      />
      <ArrayElement
        title="Arguments"
        :values="el.openOCD.args ? el.openOCD.args : []"
        :sections="['openOCD', 'args']"
        :addValue="addValueToArray"
        :removeValue="removeValueFromArray"
      />
    </div>

    <label class="is-size-4 has-text-weight-bold">Tasks</label>
    <div class="small-margin">
      <StringElement
        title="Pre Build"
        :value="el.tasks.preBuild ? el.tasks.preBuild : ''"
        :sections="['tasks', 'preBuild']"
        :updateMethod="updateElement"
      />
      <StringElement
        title="Pre Flash"
        :value="el.tasks.preFlash ? el.tasks.preFlash : ''"
        :sections="['tasks', 'preFlash']"
        :updateMethod="updateElement"
      />
      <StringElement
        title="Post Build"
        :value="el.tasks.postBuild ? el.tasks.postBuild : ''"
        :sections="['tasks', 'postBuild']"
        :updateMethod="updateElement"
      />
      <StringElement
        title="Post Flash"
        :value="el.tasks.postFlash ? el.tasks.postFlash : ''"
        :sections="['tasks', 'postFlash']"
        :updateMethod="updateElement"
      />
    </div>
  </div>
</template>

<style lang="scss">
.small-margin {
  margin-left: 2em;
  margin-bottom: 1em;
}
</style>
