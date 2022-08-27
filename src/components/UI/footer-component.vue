<template>
    <footer>
        <span>Copyright Wine Shop 2022 &copy;</span>
    </footer>
</template>

<script>
export default {
    name: 'FooterComponent',
};
</script>

<style scoped>
footer {
    display: flex;
    justify-content: center;
    font-family: var(--roboto);
    font-weight: 400;
    font-size: 0.9em;
    padding: 1em;
    color: gray;
    }
</style>
