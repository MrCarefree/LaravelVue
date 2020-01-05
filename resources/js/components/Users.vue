<template>
    <div class="container">
        <div class="row mt-5">
            <div class="col-md-12">
                <!-- card -->
                <div class="card">
                    <!-- card-header -->
                    <div class="card-header">
                        <h3 class="card-title">Users Table</h3>

                        <div class="card-tools">
                            <button class="btn btn-success" @click="newModal">
                                Add New
                                <i class="fas fa-user-plus"></i>
                            </button>
                        </div>
                    </div>
                    <!-- /.card-header -->

                    <!-- card-body -->
                    <div class="card-body table-responsive p-0">
                        <table class="table table-hover">
                            <thead>
                                <tr>
                                    <th>ID</th>
                                    <th>Name</th>
                                    <th>Email</th>
                                    <th>Type</th>
                                    <th>Registered At</th>
                                    <th>Action</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="user in users" :key="user.id">
                                    <td>{{ user.id }}</td>
                                    <td>{{ user.name }}</td>
                                    <td>{{ user.email }}</td>
                                    <td>{{ user.type | capitalized }}</td>
                                    <td>{{ user.created_at | myDate }}</td>
                                    <td>
                                        <a href="#" @click="editModal(user)">
                                            <i class="fas fa-edit blue"></i>
                                        </a>
                                        <a
                                            href="#"
                                            @click="deleteUser(user.id)"
                                        >
                                            <i class="fas fa-trash-alt red"></i>
                                        </a>
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
                        <h5
                            v-if="!editMode"
                            class="modal-title"
                            id="addNewLabel"
                        >
                            Add New
                        </h5>
                        <h5
                            v-else-if="editMode"
                            class="modal-title"
                            id="addNewLabel"
                        >
                            Update User's Info
                        </h5>
                        <button
                            type="button"
                            class="close"
                            data-dismiss="modal"
                            aria-label="Close"
                        >
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <form
                        @submit.prevent="editMode ? updateUser() : createUser"
                    >
                        <div class="modal-body">
                            <div class="form-group">
                                <input
                                    v-model="form.name"
                                    type="text"
                                    class="form-control"
                                    name="name"
                                    id="name"
                                    placeholder="Name"
                                    :class="{
                                        'is-invalid': form.errors.has('name')
                                    }"
                                />
                                <has-error
                                    :form="form"
                                    field="name"
                                ></has-error>
                            </div>
                            <div class="form-group">
                                <input
                                    v-model="form.email"
                                    type="email"
                                    class="form-control"
                                    name="email"
                                    id="email"
                                    placeholder="Email Address"
                                    :class="{
                                        'is-invalid': form.errors.has('email')
                                    }"
                                />
                                <has-error
                                    :form="form"
                                    field="email"
                                ></has-error>
                            </div>
                            <div class="form-group">
                                <textarea
                                    v-model="form.bio"
                                    class="form-control"
                                    name="bio"
                                    id="bio"
                                    placeholder="Short bio for user (optional)"
                                    :class="{
                                        'is-invalid': form.errors.has('bio')
                                    }"
                                ></textarea>
                                <has-error :form="form" field="bio"></has-error>
                            </div>
                            <div class="form-group">
                                <select
                                    name="type"
                                    v-model="form.type"
                                    id="type"
                                    class="custom-select"
                                    :class="{
                                        'is-invalid': form.errors.has('type')
                                    }"
                                >
                                    <option value>Select User Role</option>
                                    <option value="admin">Admin</option>
                                    <option value="user">Standart User</option>
                                    <option value="author">Author</option>
                                </select>
                                <has-error
                                    :form="form"
                                    field="type"
                                ></has-error>
                            </div>
                            <div class="form-group">
                                <input
                                    v-model="form.password"
                                    type="password"
                                    class="form-control"
                                    name="password"
                                    id="password"
                                    placeholder="Password"
                                    :class="{
                                        'is-invalid': form.errors.has(
                                            'password'
                                        )
                                    }"
                                />
                                <has-error
                                    :form="form"
                                    field="password"
                                ></has-error>
                            </div>
                        </div>
                        <div class="modal-footer">
                            <button
                                type="button"
                                class="btn btn-danger"
                                data-dismiss="modal"
                            >
                                Close
                            </button>
                            <button
                                v-if="editMode"
                                type="submit"
                                class="btn btn-success"
                            >
                                Update
                            </button>
                            <button
                                v-else-if="!editMode"
                                type="submit"
                                class="btn btn-primary"
                            >
                                Create
                            </button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            users: {},
            editMode: false,
            form: new Form({
                id: "",
                name: "",
                email: "",
                password: "",
                type: "",
                bio: "",
                photo: ""
            })
        };
    },

    methods: {
        loadUsers() {
            axios.get("api/user").then(({ data }) => (this.users = data.data));
        },

        newModal() {
            this.editMode = false;
            this.form.reset();
            $("#addNew").modal("show");
        },

        createUser() {
            this.$Progress.start();
            this.form
                .post("api/user")
                .then(() => {
                    Fire.$emit("AfterRequest");
                    $("#addNew").modal("hide");
                    this.$Progress.finish();
                    Toast.fire({
                        icon: "success",
                        title: "User Created Successfully"
                    });
                })
                .catch(() => {});
        },

        editModal(user) {
            this.editMode = true;
            this.form.reset();
            $("#addNew").modal("show");
            this.form.fill(user);
        },

        updateUser() {
            this.form
                .put("api/user/" + this.form.id)
                .then(() => {
                    Fire.$emit("AfterRequest");
                    $("#addNew").modal("hide");
                    this.$Progress.finish();
                    Toast.fire({
                        icon: "success",
                        title: "User Updated Successfully"
                    });
                })
                .catch(() => {
                    this.$Progress.fail();
                });
        },

        deleteUser(id) {
            Swal.fire({
                title: "Are You Sure?",
                text: "You won't be able to revert this!",
                icon: "warning",
                showCancelButton: true,
                confirmButtonColor: "#3085d6",
                cancelButtonColor: "#d33",
                confirmButtonText: "Yes, delete it!"
            }).then(result => {
                // Send request to the server
                this.form
                    .delete("api/user/" + id)
                    .then(() => {
                        if (result.value) {
                            Swal.fire(
                                "Deleted!",
                                "Your file has been deleted.",
                                "success"
                            );
                            Fire.$emit("AfterRequest");
                        }
                    })
                    .catch(() => {
                        this.$Progress.fail();
                        Swal.fire(
                            "Failed!",
                            "There is something wrong.",
                            "warning"
                        );
                    });
            });
        }
    },

    mounted() {
        this.loadUsers();
        Fire.$on("AfterRequest", () => {
            this.loadUsers();
        });
    }
};
</script>
