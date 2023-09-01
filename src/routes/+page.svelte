<script>
  import { browser } from "$app/environment";
  import { onMount } from "svelte";
  import { Navbar, NavBrand, Button, Card } from "flowbite-svelte";
  import { env } from "$env/dynamic/public";
  const {
    PUBLIC_GOOGLE_ID,
    PUBLIC_APPLE_LINK,
    PUBLIC_APP_NAME,
    PUBLIC_APP_TITLE,
    PUBLIC_APP_DEVELOPER,
    PUBLIC_COUNTDOWN,
    PUBLIC_MAIN_BTN_TEXT,
    PUBLIC_SECONDERY_BTN_TEXT,
  } = env;
  let secLeft = PUBLIC_COUNTDOWN || 3;
  let googleLink =
    "https://play.google.com/store/apps/details?id=" +
    (PUBLIC_GOOGLE_ID || "com.koodos.shelf");
  let googleIntent =
    "market://details?id=" + (PUBLIC_GOOGLE_ID || "com.koodos.shelf");
  let appleLink =
    PUBLIC_APPLE_LINK ||
    "https://apps.apple.com/us/app/shelf-whats-on-yours/id1667391175?itsct=apps_box_link&itscg=30200";

  let link = "#";

  onMount(() => {
    const IS_IOS =
      !!navigator.userAgent && /iPad|iPhone|iPod/.test(navigator.userAgent);
    const IS_MAC = !!navigator.userAgent && /Mac/.test(navigator.userAgent);

    const IS_ANDROID = !IS_IOS && navigator.userAgent.match(/android/i) != null;

    if (IS_IOS || IS_MAC) {
      link = appleLink;
    } else {
      link = googleLink;
    }

    const delay = (ms) => new Promise((res) => setTimeout(res, ms));

    const downloadBtn = document.getElementById("downloadBtn");

    const countDown = async () => {
      await delay(1000);
      secLeft--;
      if (secLeft === 0) {
        if (IS_IOS || IS_MAC) {
          link = appleLink;
          if (IS_IOS) {
            const linkFrame = document.getElementById("linkFrame");
            linkFrame.src = appleLink;
            await delay(20);
            document.body.removeChild(linkFrame);
            downloadBtn.click();
            await delay(20);
          }
        } else {
          link = googleIntent;
          await delay(20);
          downloadBtn.click();
          await delay(20);
          link = googleLink;
        }
      }
      if (secLeft >= 0) {
        await countDown();
      }
    };
    countDown();
  });
</script>

<div class="min-h-screen min-h-[100svh] grid grid-rows-[auto_1fr]">
  <Navbar class="bg-indigo-600 border-b z-50">
    <NavBrand class="text-2xl font-bold italic text-white rounded-lg" href="/"
      >{PUBLIC_APP_NAME || "shelf"}</NavBrand
    >
    <a href={link} alt={PUBLIC_APP_TITLE || "Shelf — what’s on yours?"}>
      <Button class="rounded-full"
        >{PUBLIC_SECONDERY_BTN_TEXT || "Install"}</Button
      >
    </a>
  </Navbar>
  <div class="h-full grid place-items-center content-center gap-4 pb-[12rem]">
    <Card class="grid grid-cols-[auto_1fr] gap-2 mb-6">
      <div
        class="aspect-square bg-indigo-700 text-white italic text-xl font-bold rounded-lg p-3 grid place-items-center"
      >
        {PUBLIC_APP_NAME || "shelf"}
      </div>
      <div class="p-2">
        <p class="text-gray-800 text-lg font-bold">
          {PUBLIC_APP_TITLE || "Shelf — what’s on yours?"}
        </p>
        <p class="text-green-600">
          {PUBLIC_APP_DEVELOPER || "koodos"}
        </p>
      </div>
    </Card>
    <p
      class="text-3xl
      text-gray-800"
    >
      App Store
    </p>
    <p
      class="
      text-gray-800"
    >
      {secLeft === -1
        ? "Click on the button bellow if not redirected"
        : secLeft === 0
        ? "Redirecting to app store..."
        : `Redirecting to app store in ${secLeft} seconds`}
    </p>
    <iframe id="linkFrame" width="1" height="1" style="visibility:hidden"
    ></iframe>
    <a
      href={link}
      id="downloadBtn"
      class="mt-6"
      alt={PUBLIC_APP_TITLE || "Shelf — what’s on yours?"}
      ><Button size="xl" class="rounded-full"
        >{PUBLIC_MAIN_BTN_TEXT || "Open App Store"}</Button
      ></a
    >
  </div>
</div>
