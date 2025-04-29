<script>
  import generatePayload from "promptpay-qr";
  import QRCode from "qrcode";
  import { toPng } from "html-to-image";
  import promptpayLogo from "../assets/thai-qr.svg";
  let id = $state("");
  let name = $state("");
  let qr = $state("");

  $effect(async () => {
    qr = await QRCode.toDataURL(generatePayload(id, {}), {
      width: 1000,
      height: 1000,
    });
  });

  let download = () => {
    toPng(document.getElementById("qr-card"), {
      canvasWidth: 800,
      canvasHeight: 1200,
    })
      .then((dataUrl) => {
        var link = document.createElement("a");
        link.download = "qr.png";
        link.href = dataUrl;
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
      })
      .catch((err) => {
        console.error("oops, something went wrong!", err);
      });
  };
</script>

<div class="rounded-xl overflow-hidden shadow-lg">
  <div id="qr-card" class="bg-white text-center aspect-4/6">
    <div class="bg-[#00427a] py-8 sm:py-10 p-10 relative">
      <img
        src={promptpayLogo.src}
        class="relative z-10 h-7 sm:h-10 mx-auto"
        alt=""
      />
      <div class="px-2 sm:px-6">
        <div
          class="aspect-square bg-white w-full rounded-xl -mb-40 shadow-lg mt-8 z-10 relative overflow-hidden"
        >
          {#if qr}
            <img src={qr} alt="qr code" class=" w-full" />
          {/if}
        </div>
      </div>
      <div
        class="w-[1600px] h-[1600px] absolute left-1/2 -translate-x-1/2 bottom-0 bg-[#00427a] rounded-full mx-auto -mb-14"
      ></div>
    </div>
    <div class=" flex flex-col p-10 pt-42 z-20 relative text-gray-900 gap-1">
      <input
        type="text"
        bind:value={id}
        class="text-center text-xl sm:text-2xl outline-0 [&:not(:placeholder-shown)]:text-3xl [&:not(:placeholder-shown)]:sm:text-4xl [&:not(:placeholder-shown)]:font-semibold noto hover:bg-amber-50 py-1 rounded-lg"
        placeholder="เบอร์โทร / รหัสประชาชน"
      />
      <input
        type="text"
        bind:value={name}
        class="text-center text-xl sm:text-2xl outline-0 text-gray-700 [&:not(:placeholder-shown)]:text-2xl [&:not(:placeholder-shown)]:sm:text-3xl noto py-1 rounded-lg hover:bg-amber-50"
        placeholder="ชื่อ-นามสกุล / บริษัท"
      />
    </div>
  </div>
  <div class="text-center p-8 pt-0 bg-white sm:-mt-12">
    <button
      class="inline-flex gap-3 sm:text-lg items-center bg-green-100 px-4 py-2 rounded-full cursor-pointer hover:bg-green-200"
      onclick={() => download()}
    >
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" class="h-5"
        ><path
          fill="none"
          stroke="currentColor"
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4m4-5l5 5l5-5m-5 5V3"
        /></svg
      >
      <div>Download Image</div>
    </button>
  </div>
</div>
