<template>
  <div
    class="modal fade"
    id="deleteModal"
    tabindex="-1"
    aria-labelledby="deleteModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content p-2">
        <div class="modal-body">
          <h5 class="modal-title mb-3" id="deleteModalLabel">Delete comment</h5>
          <p class="text-muted">
            Are you sure you want to delete this comment? This will remove the
            comment and can't be undone.
          </p>
          <div class="d-flex">
            <button
              type="button"
              class="btn btn-secondary px-4 py-3 me-auto"
              data-bs-dismiss="modal"
            >
              NO, CANCEL
            </button>
            <button
              type="button"
              class="btn btn-danger px-4 py-3"
              data-bs-dismiss="modal"
              @click="$emit('confirm-delete')"
            >
              YES, DELETE
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "DeleteModal",
  emits: ["confirm-delete"],
};
</script>

<style scoped>
</style>