<script>
  let { id, label, ...props } = $props()
</script>

<label class="form-label" {...props} for={id}>{label}</label>
