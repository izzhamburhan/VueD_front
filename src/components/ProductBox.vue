<template>
    <div class="column is-3">
        <div class="box">
            <figure class="image mb-4">
                <img v-bind:src="product.get_thumbnail">
            </figure>

            <h3 class="is-size-4">{{ product.name }}</h3>
            <p class="is-size-6 has-text-grey">RM{{ product.price }}</p>

            <router-link v-bind:to="product.get_absolute_url" class="button is-dark mt-4">View details</router-link>
        </div>
    </div>
</template>

<script>
export default {
    name:"ProductBox",
    props: {
        product: Object
    }
}
</script>

<style scoped>
  .image {
    border-radius: 5px; /* Rounded corners for the image */
    overflow: hidden; /* Ensure the image stays within its container */
}

.box {
    transition: transform 0.3s; /* Smooth transition for hover effect */
}

.box:hover {
    transform: translateY(-5px); /* Lift the box slightly on hover */
}

/* Additional styling for the content */
.title {
    margin-top: 0.5rem; /* Add some space above the title */
    font-weight: bold; /* Make the title bold */
}

.subtitle {
    margin-bottom: 0.5rem; /* Add some space below the subtitle */
}

.button.is-dark {
    width: 100%; /* Make the button full width */
}
</style>