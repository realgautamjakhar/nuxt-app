<template>
  <!-- Left hand side informatin -->
  <section
    class="h-screen grid lg:grid lg:grid-cols-[minmax(auto,526px)_auto] p-5 lg:p-0"
  >
    <section class="bg-[#404555] grid-rows-[auto_300px] w-auto hidden lg:grid">
      <Info />
    </section>
    <!-- Right hand side Forms  -->
    <section class="flex flex-col items-center justify-around mb-4">
      <Loginform />
    </section>
  </section>
</template>
