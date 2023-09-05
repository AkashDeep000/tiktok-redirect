<script>
  import { browser } from "$app/environment";
  import { onMount } from "svelte";
  import { Navbar, NavBrand, Button, Card } from "flowbite-svelte";
  import { env } from "$env/dynamic/public";

  const {
    PUBLIC_GOOGLE_ID,
    PUBLIC_APPLE_ID,
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
    "https://itunes.apple.com/app/" + (PUBLIC_APPLE_ID || "id1667391175");
  let appleIntents = [
    "itms-apps://itunes.apple.com/app/" + (PUBLIC_APPLE_ID || "id1667391175"),
    "itms-appss://itunes.apple.com/app/" + (PUBLIC_APPLE_ID || "id1667391175"),
    "itms-watch://itunes.apple.com/app/" + (PUBLIC_APPLE_ID || "id1667391175"),
  ];
  let link = "#";
  let IS_MAC, IS_IOS, IS_ANDROID;

  const delay = (ms) => new Promise((res) => setTimeout(res, ms));

  const handleApple = async () => {
    const a = document.createElement("a");
    document.body.appendChild(a);
    a.href = appleIntents[0];
    a.click();
    await delay(20);
    a.href = appleIntents[1];
    a.click();
    await delay(20);
    a.href = appleIntents[2];
    a.click();
    await delay(20);
    a.href = appleLink;
    a.click();
  };

  const handleOthers = async () => {
    const a = document.createElement("a");
    document.body.appendChild(a);
    a.href = googleIntent;
    a.click();
    await delay(20);
    a.href = googleLink;
    //  a.click();
  };

  const handleRedirect = async () => {
    if (IS_IOS || IS_MAC) {
      await handleApple();
    } else {
      await handleOthers();
    }
  };

  onMount(async () => {
    IS_IOS =
      !!navigator.userAgent && /iPad|iPhone|iPod/.test(navigator.userAgent);
    IS_MAC =
      !IS_IOS && !!navigator.userAgent && /Mac/.test(navigator.userAgent);
    IS_ANDROID =
      !!navigator.userAgent && !IS_IOS && /android/.test(navigator.userAgent);

    if (IS_MAC) {
      link = appleLink;
    } else if (IS_IOS) {
      link = appleIntents[0];
    } else if (IS_ANDROID) {
      link = googleIntent;
    } else {
      link = googleLink;
    }

    const countDown = async () => {
      await delay(1000);
      secLeft--;
      if (secLeft === 0) {
        await handleRedirect();
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
    <a
      on:click={handleRedirect}
      href={link}
      alt={PUBLIC_APP_TITLE || "Shelf — what’s on yours?"}
    >
      <Button class="rounded-full"
        >{PUBLIC_SECONDERY_BTN_TEXT || "Install"}</Button
      >
    </a>
  </Navbar>
  <div class="h-full grid place-items-center content-center gap-12 pb-[12rem]">
    <Card class="grid grid-cols-[auto_1fr] gap-2">
      <div
        class="aspect-square w-fit bg-indigo-700 text-white italic text-xl font-bold rounded-lg p-3 grid place-items-center"
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
      text-gray-800 px-5 text-center"
    >
      {secLeft === -1
        ? `If you're not redirected, try to open this page in your browser or search the app store for "Shelf"`
        : secLeft === 0
        ? "Redirecting to app store..."
        : `Redirecting to app store in ${secLeft} seconds`}
    </p>
    <a
      on:click={handleRedirect}
      href={link}
      class=""
      alt={PUBLIC_APP_TITLE || "Shelf — what’s on yours?"}
      ><Button size="xl" class="rounded-full"
        >{PUBLIC_MAIN_BTN_TEXT || "Open App Store"}</Button
      ></a
    >
  </div>
</div>
