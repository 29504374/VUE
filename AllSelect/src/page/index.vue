<template>
  <div id="selectAll_page">
<selectAll01 />
<selectAll02 />
  </div>
</template>
<script>
import selectAll01 from '../components/SelectAll01'
import selectAll02 from '../components/SelectAll02'
export default {
  components:{selectAll01,selectAll02}
}
</script>
<style>
#selectAll_page
{
    display: flex;
    justify-content: space-between;
    width: 100%;
    height: 100%;
}
</style>
