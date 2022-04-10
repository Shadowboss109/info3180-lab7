<template>
  <form action="" method="post" id="uploadForm"  @submit.prevent="uploadPhoto">

    
      <div class="fom-group pb-3">
        <label for="description" class="col-sm-2 col-form-label">
          Description 
        </label>

        <div class="col-sm-10">
            <textarea name="description" id="" cols="150" rows="3"></textarea>
        </div>
      </div>

      
        <div class="fom-group pb-3">
            <label for="photo" class="col-sm-2 col-form-label">
                Photo Upload: 
            </label>
            
            <div class="col-sm-10">
                <input type="file" name="photo">
            </div>
        </div>
        
        <button type="submit" class="btn btn-info">Submit</button>
    </form>
</template>

<script>
export default {
    data() {
      return {
          csrf_token: '',
          
      }        
    },
    created() {
    this.getCsrfToken();
    },
    methods: {
        getCsrfToken(){
            let self = this;
            fetch('/api/csrf-token')
                .then((response) => response.json())
                .then((data) => {
                    console.log(data);
                    self.csrf_token = data.csrf_token;
                });
        },
        uploadPhoto(){
            console.log("uploadPhoto func called");
            let uploadForm = document.querySelector("#uploadForm")
            let form_data = new FormData(uploadForm);
            fetch("/api/upload", { method: 'POST',
            body: form_data,
            headers: {'X-CSRFToken': this.csrf_token}})
            .then(function (response) {
            return response.json();
            })
            .then(function (data) {
            // display a success message
            console.log(data);
            })
            .catch(function (error) {
            console.log(error);
                    })
                }
}
}
</script>