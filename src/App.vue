<template>
  <div id="app">
    <Heading heading="Parking Lot" />
    <router-view/>
  </div>
</template>

<script>
import ParkingLot from "./components/ParkingLot";
import Heading from "./components/Heading";

export default {
  components: {
    ParkingLot,
    Heading
  }
}
</script>

<style lang="scss">
* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}
#app {
  display: flex;
  flex-direction: column;
}
</style>
