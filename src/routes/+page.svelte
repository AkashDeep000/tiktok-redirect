<script>
  import { browser } from '$app/environment'
  import { onMount } from 'svelte';
  import {
  Navbar,
  NavBrand,
  Button,
  Card
  } from 'flowbite-svelte';
  import { env } from '$env/dynamic/public';
  const {PUBLIC_GOOGLE_ID, PUBLIC_APPLE_ID, PUBLIC_APP_NAME, PUBLIC_APP_TITLE, PUBLIC_APP_DEVELOPER, PUBLIC_COUNTDOWN, PUBLIC_MAIN_BTN_TEXT, PUBLIC_SECONDERY_BTN_TEXT} = env
  let secLeft = PUBLIC_COUNTDOWN || 3;
  let link = "https://play.google.com/store/apps/details?id=" + PUBLIC_GOOGLE_ID || "com.koodos.shelf"

  onMount(() => {
  const IS_IPAD = navigator.userAgent.match(/iPad/i) != null,
  IS_MAC = navigator.userAgent.match(/Mac/i) != null,
  IS_IPHONE =
  !IS_IPAD &&
  (navigator.userAgent.match(/iPhone/i) != null ||
  navigator.userAgent.match(/iPod/i) != null),
  IS_IOS = IS_IPAD || IS_IPHONE,
  IS_ANDROID = !IS_IOS && navigator.userAgent.match(/android/i) != null,
  IS_MOBILE = IS_IOS || IS_ANDROID;

  const delay = (ms) => new Promise((res) => setTimeout(res, ms));
  const downloadBtn = document.getElementById("downloadBtn");

  if (IS_ANDROID) {
  //alert('android')
  link = "market://details?id=" + (PUBLIC_GOOGLE_ID || "com.koodos.shelf");
  } else if (IS_IOS) {
  //alert('ios')
  link = "itms-apps://itunes.apple.com/app/" + (PUBLIC_APPLE_ID || "id1667391175");
  } else if (IS_MAC) {
  //alert("mac")
  link = "https://itunes.apple.com/app/" + (PUBLIC_APPLE_ID || "id1667391175");
  } else {
  //alert("other/win")
  link = "https://play.google.com/store/apps/details?id=" + PUBLIC_GOOGLE_ID || "com.koodos.shelf"
  }

  const countDown = async () => {
  await delay(1000);
  secLeft--;
  if (secLeft === 0) {
  downloadBtn.click();
  }

  if (secLeft > 0) {
  await countDown();
  }
  };
  countDown();
  })
</script>

<div class="min-h-screen min-h-[100svh] grid grid-rows-[auto_1fr]">
  <Navbar class="bg-indigo-600 border-b z-50">
    <NavBrand class="text-2xl font-bold italic text-white rounded-lg" href="/">{PUBLIC_APP_NAME || "shelf"}</NavBrand>
    <a href={link}>
      <Button class="rounded-full">{PUBLIC_SECONDERY_BTN_TEXT || "Install"}</Button>
    </a>
  </Navbar>
  <div class="h-full grid place-items-center content-center gap-4 pb-[12rem]">
    <Card class="grid grid-cols-[auto_1fr] gap-2 mb-6">
      <div class="aspect-square bg-indigo-700 text-white italic text-xl font-bold rounded-lg p-3 grid place-items-center">
        {PUBLIC_APP_NAME || "shelf"}
      </div>
      <div class="p-2">
        <p class="text-gray-800 text-lg font-bold ">
          {PUBLIC_APP_TITLE || "Shelf — what’s on yours?"}
        </p>
        <p class="text-green-600">
          {PUBLIC_APP_DEVELOPER || "koodos"}
        </p>
      </div>
    </Card>
    <p class="text-3xl
      text-gray-800">
      App Store
    </p>
    <p class="
      text-gray-800">
      Redirecting to app store in {secLeft} seconds
    </p>
    <a href={link} id="downloadBtn" class="mt-6"><Button size="xl" class="rounded-full">{PUBLIC_MAIN_BTN_TEXT || "Open App Store"}</Button></a>
  </div>
</div>