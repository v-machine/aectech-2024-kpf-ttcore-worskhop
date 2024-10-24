<template>

    <div id="sidebar">
      <Slider :title="heightSliderName" :min="1" :max="30" :step="3" :val="heightSliderValue" @update="updateValue"></Slider>

      <Slider :title="widthSliderName" :min="1" :max="50" :step="2" :val="widthSliderValue" @update="updateValue"></Slider>

      <Slider :title="horizontalRotationSliderName" :min="0" :max="30" :step="1" :val="horizontalRotationSliderValue" @update="updateValue"></Slider>
      
      <Slider :title="verticalRotationSliderName" :min="0" :max="30" :step="1" :val="verticalRotationSliderValue" @update="updateValue"></Slider>

      <Dropdown :title="shapeDropDownName" :options="dropdownOptions" :val="shapeIndex" @update="updateValue"></Dropdown>

      <MetadataTextBox :metadata="metadata"></MetadataTextBox>
    </div>
  
    <div id="viewer">
      <GeometryView :data="inputs" :path="path" @updateMetadata="receiveMetadata"></GeometryView>
    </div>

</template>
  
<script setup>
  import { ref, onBeforeMount, computed } from "vue"
  import GeometryView from "../components/MinimalisticGeometryView.vue"
  import Slider from '../components/Slider.vue'
  import Dropdown from "../components/Dropdown.vue"
  import MetadataTextBox from "@/components/MetadataTextBox.vue"
  
  //define path to grasshopper script
  import def from "../assets/wall.gh"
  const path = def

  //define input names and values
  const heightSliderName = "Height";
  const heightSliderValue = ref(20);
  const widthSliderName = "Width";
  const widthSliderValue = ref(33);
  const horizontalRotationSliderName = "HorizontalRotation";
  const horizontalRotationSliderValue = ref(0);
  const verticalRotationSliderName = "VerticalRotation";
  const verticalRotationSliderValue = ref(0);
  const shapeDropDownName = "Shape";
  const shapeIndex = ref(0);
  const dropdownOptions = ref([
    {label: "Standard", value: 0},
    {label: "Concave", value: 1},
    {label: "Convex", value: 2},
  ]);
  const metadata=ref([]);

    //define inputs to be sent to Geometry View (and compute)
   let inputs = ref({
    [heightSliderName]: heightSliderValue.value,
    [widthSliderName]: widthSliderValue.value,
    [horizontalRotationSliderName]: horizontalRotationSliderValue.value,
    [verticalRotationSliderName]: verticalRotationSliderValue.value,
    [shapeDropDownName]: shapeIndex.value,
  });
  
  function updateValue(newValue, parameterName) {
    // Iterate over the inputs array
    for (const [key, value] of Object.entries(inputs.value)) {
      if (key == parameterName){
          inputs.value[key] = newValue
          console.log(parameterName + ':' + newValue)
      }
    }
  }

  function receiveMetadata(newValue) {
    console.log(newValue)
    metadata.value = newValue
  }
  
  </script>
  
  <style scoped>
  
  #sidebar {
    width: 310px;
    padding: 10px;
    flex-shrink: 0; 
  }
  
  #viewer { 
    width: 500px
  }
  
  </style>