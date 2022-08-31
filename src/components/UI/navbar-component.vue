<template>
    <header>
        <h1>
            <router-link to="/"> Wine Shop </router-link>
        </h1>
        <span>wineshop.com</span>
    </header>
</template>

<script>
export default {
    name: 'NavbarComponent',
};
</script>

<style scoped>
header {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: var(--black);
  height: 10%;
}

h1 {
  font-family: var(--iowan);
}

a {
  color: var(--black);
  text-decoration: none;
}

a:hover {
  color: var(--off-black);
}

span {
  font-family: var(--roboto);
  font-weight: 300;
  font-size: 0.9em;
  margin-top: -40px;
}
</style>
