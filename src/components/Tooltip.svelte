<script>
  export let data;
  export let xScale;
  export let yScale;

  console.log(xScale(data.grade));
</script>

<div class="tooltip" style="position: absolute;
    top: {yScale(data.hours)}px;
    left: {xScale(data.grade)}px; ">
<h1>{data.name}</h1>
<p>{data.hours} hours Studied</p>

</div>
<style>
  .tooltip {
    padding: 6px;
    background: withe;
    border: 1px solid rgba(0, 0, 0, 0.5);
    pointer-events: none;
    transition: top 300ms, left 380ms;
  }
  h1 {
    font-weight: 700;
    margin-bottom: 0.25rem;
  }
</style>
