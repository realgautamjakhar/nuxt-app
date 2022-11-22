<template>
  <!-- Left hand side informatin -->
  <section
    class="h-screen grid lg:grid lg:grid-cols-[minmax(auto,526px)_auto] p-4 lg:p-0"
  >
    <section class="bg-[#404555] grid-rows-[auto_300px] w-auto hidden lg:grid">
      <!-- Side Info Component (Hidden in smaller screen) -->
      <Info />
    </section>

    <!-- Right hand side Forms  -->

    <section class="flex flex-col items-center justify-around">
      <Resetpassword />
    </section>
  </section>
</template>
