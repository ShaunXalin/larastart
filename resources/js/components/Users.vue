<template>
  <div class="container">
    <div class="row mt-5">
      <div class="col-md-12">
        <div class="card">
          <div class="card-header">
            <h3 class="card-title">Users Table</h3>

            <div class="card-tools">
              <button class="btn btn-primary" @click="newModal">
                Add New
                <i class="fas fa-user-plus fa-fw"></i>
              </button>
            </div>
          </div>
          <!-- /.card-header -->
          <div class="card-body table-responsive p-0">
            <table class="table table-hover">
              <thead>
                <tr>
                  <th>ID</th>
                  <th>Name</th>
                  <th>Email</th>
                  <th>Type</th>
                  <th>Created At</th>
                  <th>Modify</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="user in users" :key="user.id">
                  <td>{{user.id}}</td>
                  <td>{{user.name}}</td>
                  <td>{{user.email}}</td>
                  <td>
                    <span class="tag tag-primary">{{user.type | upText}}</span>
                  </td>
                  <td>{{user.created_at | myDate}}</td>
                  <td>
                    <!-- <a @click="editModal(user)"> -->
                      <i class="fa fa-edit blue"></i>
                    <!-- </a> -->
                    /
                    <!-- <a @click="deleteUser(user.id)"> -->
                      <i class="fa fa-trash red"></i>
                    <!-- </a> -->
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
          <!-- /.card-body -->
        </div>
        <!-- /.card -->
      </div>
    </div>
    <!-- Modal -->
    <div
      class="modal fade"
      id="addNew"
      tabindex="-1"
      role="dialog"
      aria-labelledby="addNewLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog modal-dialog-centered" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="addNewLabel">Add New User</h5>
            <!-- <h5 class="modal-title" v-show="!editmode" id="addNewLabel">Add New User</h5> -->
            <!-- <h5 class="modal-title" v-show="editmode" id="addNewLabel">Update User Info</h5> -->
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <form @submit.prevent="editmode ? updateUser() : createUser()">
            <div class="modal-body">
              <div class="form-group">
                <input
                  v-model="form.name"
                  placeholder="Name"
                  type="text"
                  name="name"
                  class="form-control"
                  :class="{'is-invalid': form.errors.has('name')}"
                />
                <has-error :form="form" field="name"></has-error>
              </div>
              <div class="form-group">
                <input
                  v-model="form.email"
                  placeholder="Email-id"
                  type="text"
                  name="email"
                  class="form-control"
                  :class="{'is-invalid': form.errors.has('email')}"
                />
                <has-error :form="form" field="email"></has-error>
              </div>
              <div class="form-group">
                <input
                  v-model="form.bio"
                  placeholder="enter your bio"
                  id="bio"
                  name="bio"
                  class="form-control"
                  :class="{'is-invalid': form.errors.has('bio')}"
                />
                <has-error :form="form" field="bio"></has-error>
              </div>
              <div class="form-group">
                <select
                  v-model="form.type"
                  placeholder="name"
                  id="type"
                  name="type"
                  class="form-control"
                  :class="{'is-invalid': form.errors.has('type')}"
                >
                  <option value>Select User role</option>
                  <option value="admin">Admin</option>
                  <option value="user">Standard User</option>
                  <option value="author">Author</option>
                </select>
                <has-error :form="form" field="type"></has-error>
              </div>
              <div class="form-group">
                <input
                  v-model="form.password"
                  placeholder="Password"
                  id="password"
                  type="password"
                  name="password"
                  class="form-control"
                  :class="{'is-invalid': form.errors.has('password')}"
                />
                <has-error :form="form" field="password"></has-error>
              </div>
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
              <!-- <button v-show="editmode" type="submit" class="btn btn-success">Update</button> -->
              <button v-show="!editmode" type="submit" class="btn btn-primary">Create</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  // https://github.com/cretueusebiu/vform
  data() {
    return {
      // editmode: false,
      // users: {},
      form: new Form({
        id: "",
        name: "",
        email: "",
        password: "",
        type: "",
        bio: "",
        photo: "",
      }),
    };
  },
  methods: {
    // updateUser(){
    //   this.$Progress.start();
    //   console.log('Editing data');
    //   this.form.put('api/user/'+this.form.id)
    //   .then(()=>{
    //     $('#addNew').modal('hide');
    //     swal.fire(
    //       'Updated!',
    //       'Information has been updated',
    //       'success'
    //     )
    //     Fire.$emit("Reload");
    //     this.$progress.finish();      
    //   })
    //   .catch(()=> {
    //     this.$Progress.fail();
    //   })
    // },
    // editModal(user) {
    //   this.editmode = true;
    //   this.form.reset();
    //   $("#addNew").modal("show");
    //   this.form.fill(user);
    // },
    newModal() {
      this.editmode = false;
      this.form.reset();
      $("#addNew").modal("show");
    },
    // deleteUser(id) {
    //   swal
    //     .fire({
    //       title: "Are you sure?",
    //       text: "You won't be able to revert this!",
    //       type: "warning",
    //       showCancelButton: true,
    //       confirmButtonColor: "#3085d6",
    //       cancelButtonColor: "#d33",
    //       confirmButtonText: "Yes, delete it!",
    //     })
    //     .then((result) => {
    //       //Send request to the server
    //       if (result.value) {
    //         this.form
    //           .delete("api/user/" + id)
    //           .then(() => {
    //             swal.fire("Deleted!", "Your id has been deleted", "success");
    //             Fire.$emit("Reload");
    //           })
    //           .catch(() => {
    //             swal.fire("Failed!", "There was something wrong", "warning");
    //           });
    //       }
    //     });
    // },
    // loadUsers() {
    //   axios.get("api/user").then(({ data }) => (this.users = data.data));
    // },
    createUser() {
      // this.$Progress.start();
      this.form.post("api/user").then(() => {
        Fire.$emit("Reload");
        $("#addNew").modal("hide");
        toast.fire({
          type: "success",
          title: "User created successfully",
        });
        this.$Progress.finish();
      });
    },
  },
  // created() {
  //   this.loadUsers();
  //   Fire.$on("Reload", () => {
  //     this.loadUsers();
  //   });
  //   // setInterval(() => this.loadUsers(), 3000);
  // },
};
</script>