<script lang="ts">
  let nameInput = $state("");
  let numberInput = $state("");
  let monthInput = $state("");
  let yearInput = $state("");
  let cvcInput = $state("");
  let confirm = $state(false);
  let numberInputError = $state(false);
  let nameInputError = $state(false);
  let monthInputError = $state(false);
  let yearInputError = $state(false);
  let cvcInputError = $state(false);
  function handleConfirm() {
    // reset errors
    nameInputError = false;
    numberInputError = false;
    monthInputError = false;
    yearInputError = false;
    cvcInputError = false;

    // NAME (letters only)
    if (!/^[A-Za-z ]+$/.test(nameInput)) {
      nameInputError = true;
      return;
    }

    // CARD NUMBER (16 digits)
    if (!/^\d{16}$/.test(numberInput.replace(/\s/g, ""))) {
      numberInputError = true;
      return;
    }

    // MONTH (01–12)
    if (!/^(0[1-9]|1[0-2])$/.test(monthInput)) {
      monthInputError = true;
      return;
    }

    // YEAR (2 digits)
    if (!/^\d{2}$/.test(yearInput)) {
      yearInputError = true;
      return;
    }

    // CVC (3 digits)
    if (!/^\d{3}$/.test(cvcInput)) {
      cvcInputError = true;
      return;
    }

    confirm = true;
  }
</script>

<div class="xl:flex lg:justify-between fixed w-full">
  <!--bg -->

  <img
    src="/images/bg-main-mobile.png"
    alt="bg-mobile"
    class="w-full h-60 object-cover relative z-0 lg:hidden"
  />
  <img
    src="/images/bg-main-desktop.png"
    alt="bg-desktop"
    class="h-screen w-110 object-cover relative z-0 hidden lg:flex"
  />
  <!--back card-->

  <img
    src="/images/bg-card-back.png"
    alt="back-card"
    class="absolute z-10 w-75 h-40 lg:w-100 lg:h-60 top-10 right-5 lg:top-120 lg:left-70"
  />
  <p
    class="text-white absolute top-27 right-14 font-space z-20 text-xs lg:top-146 lg:text-lg lg:left-150"
  >
    {cvcInput ? cvcInput.replace(/\D/g, "") : "000"}
  </p>
  <!--front card-->
  <img
    src="/images/bg-card-front.png"
    alt="front-card"
    class="absolute z-20 w-75 lg:w-100 lg:h-60 h-40 left-5 top-35 lg:left-40 lg:top-55"
  />
  <img
    src="/images/card-logo.png"
    alt="logo"
    class="absolute z-30 top-40 w-14 h-8 left-12 lg:left-47 lg:top-60"
  />
  <div
    class="text-white font-space absolute z-30 top-55 left-12 lg:top-90 lg:left-50 w-60 lg:w-80"
  >
    <div class="text-lg lg:text-2xl tracking-widest pb-5 w-full">
      {numberInput
        ? numberInput
            .replace(/\D/g, "")
            .replace(/(.{4})/g, "$1 ")
            .trim()
        : "0000 0000 0000 0000"}
    </div>

    <div class="flex justify-between text-xs lg:text-md w-full">
      {nameInput ? nameInput : "JANE APPLESEED"}
      <div class="flex">
        {monthInput ? monthInput.padStart(2, "0") : "00"}/
        {yearInput ? yearInput : "00"}
      </div>
    </div>
  </div>

  <!--form-->

  <div class="pt-30 px-5 font-space space-y-5 lg:ml-100 lg:mr-70 lg:mt-30">
    {#if !confirm}
      <!--name-->
      <div class="space-y-2">
        <h1 class=" text-xs text-[#230c39]">Cardholder name</h1>
        <input
          placeholder="e.g.Jane Appleseed"
          type="text"
          bind:value={nameInput}
          class={` ${nameInput ? "text-[#230c39] border-indigo-950" : nameInputError ? "border-red-500 " : "border-mist-400 text-mist-400"} border  w-full text-sm cursor-pointer rounded-md p-3`}
        />
        {#if nameInputError}
          <p class="text-red-500 text-xs">invalid Name.</p>{/if}
      </div>
      <!--number-->
      <div class="space-y-2">
        <h1 class=" text-xs text-[#230c39]">Card number</h1>
        <input
          placeholder="e.g.1234 6789 1230 0000"
          type="tel"
          maxlength="16"
          bind:value={numberInput}
          class={` ${numberInput ? "text-[#230c39] border-indigo-950" : numberInputError ? "border-red-500" : "border-mist-400 text-mist-400"} border  w-full text-sm cursor-pointer rounded-md p-3`}
        />
        {#if numberInputError}
          <p class="text-red-500 text-xs">Wrong format.numbers only.</p>{/if}
      </div>
      <!--date and cvc-->

      <div class="space-y-2">
        <div class="flex justify-start gap-20">
          <h1 class=" text-xs text-[#230c39]">Exp. date(mm/yy)</h1>
          <h1 class=" text-xs text-[#230c39]">CVC</h1>
        </div>
        <div class="grid grid-cols-4 gap-4">
          <!---mm-->
          <div class="col-span-1">
            <input
              placeholder="MM"
              type="text"
              maxlength="2"
              bind:value={monthInput}
              class={` ${monthInput ? "text-[#230c39] border-indigo-950" : monthInputError ? "border-red-500" : "border-mist-400 text-mist-400"} border  w-full text-sm cursor-pointer rounded-md p-3`}
            />
            {#if monthInputError}
              <p class="text-red-500 text-xs">Can't be blank</p>{/if}
          </div>
          <!---yy-->
          <div class="col-span-1">
            <input
              placeholder="YY"
              type="text"
              maxlength="2"
              bind:value={yearInput}
              class={` ${yearInput ? "text-[#230c39] border-indigo-950" : yearInputError ? "border-red-500" : "border-mist-400 text-mist-400"} border  w-full text-sm cursor-pointer rounded-md p-3`}
            />
            {#if yearInputError}
              <p class="text-red-500 text-xs">Can't be blank</p>{/if}
          </div>
          <!---cvc-->
          <div class="col-span-2">
            <input
              placeholder="e.g.123"
              type="text"
              maxlength="3"
              bind:value={cvcInput}
              class={` ${cvcInput ? "text-[#230c39] border-indigo-950" : cvcInputError ? "border-red-500" : "border-mist-400 text-mist-400"} border  w-full  text-sm cursor-pointer rounded-md p-3`}
            />
            {#if cvcInputError}
              <p class="text-red-500 text-xs">Can't be blank</p>{/if}
          </div>
        </div>
      </div>
      <!--confirm-->
      <button
        onclick={handleConfirm}
        class="border font-normal rounded-lg w-full py-4 bg-[#230c39] text-center text-white hover:bg-[#2d2238] mt-5"
        >Confirm</button
      >
    {:else}
      <div class="flex-col justify-center items-center text-center w-full">
        <img
          src="/images/icon-complete.png"
          alt="complete icon"
          class="w-22 h-22 absolute z-40 left-40 bottom-70 lg:left-245 lg:top-60"
        />
        <div class="pt-30 w-full">
          <h1 class=" font-space pb-5 text-3xl text-[#230c39]">thank you!</h1>
          <h2 class="text-gray-400 font-mono text-lg lg:text-sm">
            We've added your card details
          </h2>
          <!--continue-->
          <button
            onclick={() => (confirm = false)}
            class="border mt-22 lg:mt-10 font-normal rounded-lg w-full py-4 bg-[#230c39] text-center text-white hover:bg-[#2d2238]"
            >Continue</button
          >
        </div>
      </div>
    {/if}
  </div>
</div>
