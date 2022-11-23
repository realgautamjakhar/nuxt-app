<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800;900&display=swap");

:root {
  font-family: "Poppins", sans-serif;
}
</style>

<template>
  <!-- Left hand side informatin -->
  <section
    class="h-screen grid lg:grid lg:grid-cols-[minmax(auto,526px)_auto] p-4 lg:p-0"
  >
    <section class="bg-[#404555] grid-rows-[auto_300px] w-auto hidden lg:grid">
      <Info />
    </section>
    <!-- Right hand side Forms  -->
    <section class="flex flex-col items-center justify-around">
      <Loginform />
    </section>
  </section>
</template>
