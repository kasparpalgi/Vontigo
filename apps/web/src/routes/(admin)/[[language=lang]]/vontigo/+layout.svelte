<script lang="ts">
	// Your selected Skeleton theme:
	// import '@skeletonlabs/skeleton/themes/theme-crimson.css';
	import './vontigo.css';
	// This contains the bulk of Skeletons required styles:
	import '@skeletonlabs/skeleton/styles/all.css';
	// Finally, your application's global stylesheet (sometimes labeled 'app.css')
	import './app.postcss';

	import { signOut } from '@auth/sveltekit/client';

	import {
		AppShell,
		Avatar,
		ProgressBar,
		ProgressRadial,
		Toast,
		filter,
		popup
	} from '@skeletonlabs/skeleton';
	import { LightSwitch } from '@skeletonlabs/skeleton';
	import { page, navigating } from '$app/stores';
	import { PUBLIC_DEFAULT_LANG } from '$env/static/public';
	import {
		language,
		origin,
		siteUrl,
		adminSiteUrl,
		isSignedIn
	} from '$lib/core/shared/stores/site';
	import IconHome from '$lib/icons/IconHome.svelte';
	import IconWindow from '$lib/icons/IconWindow.svelte';
	import IconContent from '$lib/icons/IconContent.svelte';
	import Icon3BottomLeft from '$lib/icons/Icon3BottomLeft.svelte';
	import IconTag from '$lib/icons/IconTag.svelte';
	// import IconUsers from '$lib/icons/IconUsers.svelte';

	import { computePosition, autoUpdate, flip, shift, offset, arrow } from '@floating-ui/dom';
	import { storePopup, type PopupSettings } from '@skeletonlabs/skeleton';
	// import IconDatabase from '$lib/icons/IconDatabase.svelte';
	import CompGravatar from '$lib/core/core/frontend/components/shared/CompGravatar/CompGravatar.svelte';

	import { fade } from 'svelte/transition';
	import { onMount } from 'svelte';

	let visible = false;

	onMount(() => {
		visible = true;
	});

	storePopup.set({ computePosition, autoUpdate, flip, shift, offset, arrow });
	origin.set($page.url.host);

	let href: string;

	origin.set($page.url.host);

	let popupSettings: PopupSettings = {
		// Set the event as: click | hover | hover-click | focus | focus-click
		event: 'click',
		// Provide a matching 'data-popup' value.
		target: 'examplePopup'
	};

	if ($page.params) {
		language.set($page.params.language ? $page.params.language : PUBLIC_DEFAULT_LANG);
		adminSiteUrl.set($page.params.language ? '/' + $page.params.language + '/vontigo' : '/vontigo');
	}

	$: classesActive = (href: string) =>
		$page.url.pathname.indexOf(href) > -1 ? '!bg-primary-500 text-white' : '';
</script>

{#if visible}
	<div transition:fade class="h-full w-full">
		<AppShell regionPage="relative" slotPageHeader="sticky top-0 z-10 ">
			<!-- <svelte:fragment slot="header">Header</svelte:fragment> -->
			<svelte:fragment slot="sidebarLeft">
				{#if $isSignedIn}
					<div
						id=""
						class="sidebar-left hidden lg:block text-lg border-e border-gray-300 h-full min-w-[300px]"
					>
						<nav class="list-nav space-y-4 h-full flex flex-col">
							<!-- (optionally you can provde a label here) -->
							<ul class="p-2 pl-10 px-6 pt-6">
								<li class="w-auto">
									<span class="unstyled flex-auto font-bold antialiased"
										><span class="font-black text-4xl text-primary-500">V</span>ontigo</span
									>
								</li>
							</ul>
							<ul class="p-2 px-6 pt-0">
								<li class="w-auto">
									<a
										class={classesActive($adminSiteUrl + '/dashboard')}
										href={$adminSiteUrl + '/dashboard'}
									>
										<IconHome />
										<span class="flex-auto text-base">Dashboard</span>
									</a>
								</li>
								<li class="w-auto flex gap-2">
									<a
										class="w-full {classesActive($adminSiteUrl + '/site')}"
										href={$adminSiteUrl + '/site'}
									>
										<IconWindow />
										<span class="flex-auto text-base">View site</span>
									</a>
									<a data-sveltekit-reload class="p-2" href="/" target="_blank">
										<svg
											xmlns="http://www.w3.org/2000/svg"
											fill="none"
											viewBox="0 0 24 24"
											stroke-width="2"
											stroke="currentColor"
											class="w-6 h-5"
										>
											<path
												stroke-linecap="round"
												stroke-linejoin="round"
												d="M13.5 6H5.25A2.25 2.25 0 003 8.25v10.5A2.25 2.25 0 005.25 21h10.5A2.25 2.25 0 0018 18.75V10.5m-10.5 6L21 3m0 0h-5.25M21 3v5.25"
											/>
										</svg>
									</a>
								</li>
							</ul>
							<ul class="p-2 px-6">
								<li class="w-auto">
									<ul>
										<li class="w-auto">
											<a
												class={classesActive($adminSiteUrl + '/posts/post/page')}
												href={$adminSiteUrl + '/posts/post/page/1'}
											>
												<Icon3BottomLeft />
												<span class="flex-auto text-base">Posts</span>
											</a>
										</li>
										<li class="w-auto">
											<a
												class={classesActive($adminSiteUrl + '/posts/post/drafts/page')}
												href={$adminSiteUrl + '/posts/post/drafts/page/1'}
											>
												<span class="pl-10 flex-auto text-sm">Drafts</span>
											</a>
										</li>
										<li class="w-auto">
											<a
												class={classesActive($adminSiteUrl + '/posts/post/scheduled/page')}
												href={$adminSiteUrl + '/posts/post/scheduled/page/1'}
											>
												<!-- <span class="badge bg-primary-500 w-10 h-10 bg-none">LOGO</span> -->
												<span class="pl-10 flex-auto text-sm">Scheduled</span>
											</a>
										</li>
										<li class="w-auto">
											<a
												class={classesActive($adminSiteUrl + '/posts/post/published/page')}
												href={$adminSiteUrl + '/posts/post/published/page/1'}
											>
												<!-- <span class="badge bg-primary-500 w-10 h-10 bg-none">LOGO</span> -->
												<span class="pl-10 flex-auto text-sm">Published</span>
											</a>
										</li>
									</ul>
								</li>
							</ul>
							<ul class="p-2 px-6">
								<li class="w-auto">
									<a
										class={classesActive($adminSiteUrl + '/posts/page')}
										href={$adminSiteUrl + '/posts/page/page/1'}
									>
										<IconContent />
										<span class="flex-auto text-base">Pages</span>
									</a>
								</li>
								<li class="w-auto">
									<a
										class={classesActive($adminSiteUrl + '/tags/public')}
										href={$adminSiteUrl + '/tags/public'}
									>
										<IconTag />
										<span class="flex-auto text-base">Tags</span>
									</a>
								</li>
								<!-- <li class="w-auto">
						<a class={classesActive($adminSiteUrl + '/members')} href={$adminSiteUrl + '/members'}>
							<IconUsers />
							<span class="flex-auto text-base">Members</span>
						</a>
					</li> -->
								<!-- <li class="w-auto">
						<a class={classesActive($adminSiteUrl + '/base')} href={$adminSiteUrl + '/base'}>
							<IconDatabase />
							<span class="flex-auto text-base">Vontigobase</span>
						</a>
					</li> -->
							</ul>
							<ul class="h-full" />
							<ul class="sidebar-left__bottom p-8 flex flex-row">
								<li class="w-full mt-1 flex">
									<div
										class=" relative inline-block my-auto cursor-pointer"
										use:popup={popupSettings}
									>
										<span
											class="badge-icon bg-green-400 absolute -top-0 -right-0 z-10 w-2 h-2 border"
										/>
										{#if $page?.data?.session?.user?.profile_image}
											<Avatar
												src={$page?.data?.session?.user?.profile_image}
												action={filter}
												actionParams="#Apollo"
												width="w-8"
											/>
										{:else}
											<CompGravatar
												email={$page?.data?.session?.user?.email}
												size={32}
												default="robohash"
												class="rounded-full"
											/>
										{/if}
									</div>
								</li>
								<li class="w-full" />
								<li class="w-auto text-end content-end settings">
									<a
										class={classesActive($adminSiteUrl + '/settings') + ' mr-2'}
										href={$adminSiteUrl + '/settings'}
									>
										<svg
											xmlns="http://www.w3.org/2000/svg"
											fill="none"
											viewBox="0 0 24 24"
											stroke-width="1"
											stroke="currentColor"
											class="w-6 h-6"
										>
											<path
												stroke-linecap="round"
												stroke-linejoin="round"
												d="M9.594 3.94c.09-.542.56-.94 1.11-.94h2.593c.55 0 1.02.398 1.11.94l.213 1.281c.063.374.313.686.645.87.074.04.147.083.22.127.324.196.72.257 1.075.124l1.217-.456a1.125 1.125 0 011.37.49l1.296 2.247a1.125 1.125 0 01-.26 1.431l-1.003.827c-.293.24-.438.613-.431.992a6.759 6.759 0 010 .255c-.007.378.138.75.43.99l1.005.828c.424.35.534.954.26 1.43l-1.298 2.247a1.125 1.125 0 01-1.369.491l-1.217-.456c-.355-.133-.75-.072-1.076.124a6.57 6.57 0 01-.22.128c-.331.183-.581.495-.644.869l-.213 1.28c-.09.543-.56.941-1.11.941h-2.594c-.55 0-1.02-.398-1.11-.94l-.213-1.281c-.062-.374-.312-.686-.644-.87a6.52 6.52 0 01-.22-.127c-.325-.196-.72-.257-1.076-.124l-1.217.456a1.125 1.125 0 01-1.369-.49l-1.297-2.247a1.125 1.125 0 01.26-1.431l1.004-.827c.292-.24.437-.613.43-.992a6.932 6.932 0 010-.255c.007-.378-.138-.75-.43-.99l-1.004-.828a1.125 1.125 0 01-.26-1.43l1.297-2.247a1.125 1.125 0 011.37-.491l1.216.456c.356.133.751.072 1.076-.124.072-.044.146-.087.22-.128.332-.183.582-.495.644-.869l.214-1.281z"
											/>
											<path
												stroke-linecap="round"
												stroke-linejoin="round"
												d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"
											/>
										</svg>
									</a>
								</li>
								<li class="w-auto text-end content-end py-2">
									<span class="flex-auto text-base">
										<LightSwitch height="h-5" width="w-10" rounded="rounded-full" />
									</span>
								</li>
							</ul>
						</nav>
					</div>

					<div class="card p-2 shadow text-base" data-popup="examplePopup">
						<nav class="list-nav">
							<!-- (optionally you can provide a label here) -->
							<ul>
								<li>
									<a
										class={classesActive($adminSiteUrl + '/settings/staff')}
										href={$adminSiteUrl + '/settings/staff/' + $page?.data?.session?.user?.slug}
									>
										<svg
											xmlns="http://www.w3.org/2000/svg"
											fill="none"
											viewBox="0 0 24 24"
											stroke-width="1.5"
											stroke="currentColor"
											class="w-5 h-5"
										>
											<path
												stroke-linecap="round"
												stroke-linejoin="round"
												d="M17.982 18.725A7.488 7.488 0 0012 15.75a7.488 7.488 0 00-5.982 2.975m11.963 0a9 9 0 10-11.963 0m11.963 0A8.966 8.966 0 0112 21a8.966 8.966 0 01-5.982-2.275M15 9.75a3 3 0 11-6 0 3 3 0 016 0z"
											/>
										</svg>

										<span class="flex-auto">My profile</span>
									</a>
								</li>
								<li>
									<a class={classesActive($adminSiteUrl + '/docs')} href={$adminSiteUrl + '/docs'}>
										<span class="">
											<svg
												xmlns="http://www.w3.org/2000/svg"
												fill="none"
												viewBox="0 0 24 24"
												stroke-width="1.5"
												stroke="currentColor"
												class="w-5 h-5"
											>
												<path
													stroke-linecap="round"
													stroke-linejoin="round"
													d="M4.26 10.147a60.436 60.436 0 00-.491 6.347A48.627 48.627 0 0112 20.904a48.627 48.627 0 018.232-4.41 60.46 60.46 0 00-.491-6.347m-15.482 0a50.57 50.57 0 00-2.658-.813A59.905 59.905 0 0112 3.493a59.902 59.902 0 0110.399 5.84c-.896.248-1.783.52-2.658.814m-15.482 0A50.697 50.697 0 0112 13.489a50.702 50.702 0 017.74-3.342M6.75 15a.75.75 0 100-1.5.75.75 0 000 1.5zm0 0v-3.675A55.378 55.378 0 0112 8.443m-7.007 11.55A5.981 5.981 0 006.75 15.75v-1.5"
												/>
											</svg>
										</span>
										<span class="flex-auto">Developer docs</span>
									</a>
								</li>
								<li>
									<a href="#" on:click={() => signOut()} class="p-1">
										<span class=""
											><svg
												xmlns="http://www.w3.org/2000/svg"
												fill="none"
												viewBox="0 0 24 24"
												stroke-width="1.5"
												stroke="currentColor"
												class="w-5 h-5"
											>
												<path
													stroke-linecap="round"
													stroke-linejoin="round"
													d="M5.636 5.636a9 9 0 1012.728 0M12 3v9"
												/>
											</svg>
										</span>
										<span class="flex-auto">Signout</span>
									</a>
								</li>
								<!-- ... -->
							</ul>
						</nav>
					</div>
				{/if}
			</svelte:fragment>
			<!-- <svelte:fragment slot="sidebarRight">Sidebar Right</svelte:fragment> -->
			<!-- <svelte:fragment slot="pageHeader">
		<AppBar class="text-3xl font-bold">Dashboard</AppBar>
	</svelte:fragment> -->
			<!-- Router Slot -->

			{#if $navigating}
				<ProgressBar height={'h-1'} class="absolute top-0 w-full z-20" />
			{:else}
				<slot />
			{/if}

			<!-- ---- / ---- -->
			<!-- <svelte:fragment slot="pageFooter">Page Footer</svelte:fragment> -->
			<!-- <svelte:fragment slot="footer">Footer</svelte:fragment> -->
		</AppShell>
	</div>
{:else}
	<div class="w-full h-full flex">
		<div class="m-auto">
			<ProgressRadial ... stroke={180} meter="stroke-primary-500" track="stroke-primary-500/30" />
		</div>
	</div>
{/if}
<Toast position={'tr'} width={'max-w-[350px]'} />
