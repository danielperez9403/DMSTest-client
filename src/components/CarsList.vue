<template>
    <div class="post">
        <div v-if="loading" class="loading d-flex justify-content-center mt-5 fs-4">
            <span>Loading...</span>
        </div>

        <div v-if="cars" class="content">
            <div class="row justify-content-center mt-4 mb-4">
                <div class="row col-10">
                    <div class="col-lg-3 col-md-4 col-sm-5 col-6 px-0 me-3 mb-md-0 mb-3">
                        <label for="dealer" class="ms-1 mb-1 fw-bold"> Dealer: </label>
                        <select id="dealer" class="form-select" v-model="dealer">
                            <option>Dealer 1</option>
                            <option>Dealer 2</option>
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
                                <td>{{ car.brand }}</td>
                                <td>{{ car.model }}</td>
                                <td>{{ car.price }}</td>
                                <td>{{ car.mileage }}</td>
                                <td>{{ car.color }}</td>
                                <td>{{ car.status }}</td>
                            </tr>
                        </tbody>
                    </table>
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
                dealer: "Dealer 1",
                sorting: ""
            };
        },
        created() {
            // fetch the data when the view is created and the data is
            // already being observed
            this.fetchData();
        },
        watch: {
            // call again the method if the route changes
            '$route': 'fetchData',
            'dealer': 'fetchData',
            'sorting': 'sortData'
        },
        methods: {
            fetchData() {
                this.cars = null;
                this.loading = true;
                fetch(`api/cardealer?dealer=${this.dealer === "Dealer 1" ? "Dealer1" : "Dealer2"}`)
                    .then(r => r.json())
                    .then(json => {
                        this.cars = json;
                        this.loading = false;
                        return;
                    });
            },
            sortData() {
                if (this.sorting === 'brand') {
                    this.cars.sort((a, b) => {
                        if (a.brand > b.brand) {
                            return 1;
                        } else if (a.brand < b.brand) {
                            return -1;
                        } else {
                            return a.price - b.price;
                        }
                    });
                } else if (this.sorting === 'model') {
                    this.cars.sort((a, b) => {
                        if (a.model > b.model) {
                            return 1;
                        } else if (a.model < b.model) {
                            return -1;
                        } else {
                            return a.price - b.price;
                        }
                    });
                } else if (this.sorting === 'price_low_high') {
                    this.cars.sort((a, b) => {
                        return a.price - b.price;
                    });
                } else if (this.sorting === 'price_high_low') {
                    this.cars.sort((a, b) => {
                        return b.price - a.price;
                    });
                }
                
            }
        },
    });
</script>