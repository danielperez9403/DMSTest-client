<template>
    <div class="post">
        <div v-if="error" class="content">
            <div class="d-flex justify-content-center mt-5">
                <h2>ERROR {{ error.status }}</h2>
            </div>
            <div class="d-flex justify-content-center mt-2 fs-4">
                <p v-if="error.status === 404">The requested resource was not found.</p>
                <p v-else-if="error.status === 500">There was a server error.</p>
                <p v-else>An unknown error occurred.</p>
            </div>
        </div>
        <div v-else>
            <div v-if="loading" class="loading d-flex justify-content-center mt-5 fs-4">
                <span>Loading...</span>
            </div>

            <div v-if="cars" class="content">
                <div class="row justify-content-center mt-4 mb-4">
                    <div class="row col-10">
                        <div class="col-lg-3 col-md-4 col-sm-5 col-6 px-0 me-3 mb-md-0 mb-3">
                            <label for="dealer" class="ms-1 mb-1 fw-bold"> Dealer: </label>
                            <select id="dealer" class="form-select" v-model="dealerId">
                                <option value="1">Dealer 1</option>
                                <option value="2">Dealer 2</option>
                            </select>
                        </div>
                        <div class="col-lg-3 col-md-4 col-sm-5 col-6 px-0">
                            <label for="sorting" class="ms-1 mb-1 fw-bold"> Sort by: </label>
                            <select id="sorting" class="form-select" v-model="sorting">
                                <option selected disabled value=""> - Select an option - </option>
                                <option value="brand">Brand (A - Z)</option>
                                <option value="model">Model (A - Z)</option>
                                <option value="price_low_high">Price (Low to High)</option>
                                <option value="price_high_low">Price (High to Low)</option>
                            </select>
                        </div>
                    </div>
                </div>
                <div class="row justify-content-center">
                    <div class="col-10 table-responsive">
                        <table class="table table-hover">
                            <thead>
                                <tr>
                                    <th>Brand</th>
                                    <th>Model</th>
                                    <th>Price</th>
                                    <th>Mileage</th>
                                    <th>Color</th>
                                    <th>Status</th>
                                </tr>
                            </thead>

                            <tbody>
                                <tr v-for="(car, index) in cars" :key="index">
                                    <td>{{ car.Brand }}</td>
                                    <td>{{ car.Model }}</td>
                                    <td>{{ car.Price }}</td>
                                    <td>{{ car.Mileage }}</td>
                                    <td>{{ car.Color }}</td>
                                    <td>{{ car.Dealer.Status }}</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="js">
    import { defineComponent } from 'vue';

    export default defineComponent({
        data() {
            return {
                loading: false,
                cars: null,
                dealerId: 1,
                sorting: "",
                error: null
            };
        },
        created() {
            this.fetchData();
        },
        watch: {
            // call again the method if the route changes
            '$route': 'fetchData',
            'dealerId': 'fetchData',
            'sorting': 'sortData'
        },
        methods: {
            async fetchData() {
                this.cars = null;
                this.loading = true;

                await fetch(`api/cardealer?dealerId=${this.dealerId}`)
                    .then(r => {
                        if (!r.ok) {
                            throw r;
                        }

                        return r.json();
                    })
                    .then(json => {
                        this.cars = json.cars;
                        this.loading = false;
                        return;
                    })
                    .catch(error => {
                        this.handleError(error);
                    });
            },
            sortData() {
                if (this.sorting === 'brand') {
                    this.cars.sort((a, b) => {
                        if (a.Brand > b.Brand) {
                            return 1;
                        } else if (a.Brand < b.Brand) {
                            return -1;
                        } else {
                            return a.Price - b.Price;
                        }
                    });
                } else if (this.sorting === 'model') {
                    this.cars.sort((a, b) => {
                        if (a.Model > b.Model) {
                            return 1;
                        } else if (a.Model < b.Model) {
                            return -1;
                        } else {
                            return a.Price - b.price;
                        }
                    });
                } else if (this.sorting === 'price_low_high') {
                    this.cars.sort((a, b) => {
                        return a.Price - b.Price;
                    });
                } else if (this.sorting === 'price_high_low') {
                    this.cars.sort((a, b) => {
                        return b.Price - a.Price;
                    });
                }
            },
            handleError(error) {
                this.error = {
                    status: error.status,
                };
            },
        },
    });
</script>