<script>
	import { CMS_URL } from '$lib/globals.js';
	import Breadcrumbs from '$lib/components/ui/Breadcrumbs.svelte';
	import { Splide, SplideSlide } from '@splidejs/svelte-splide';
	import '@splidejs/svelte-splide/css/core';
	import SheetContent from '$lib/components/ui/SheetContent.svelte';
	import CatalogRow from '$lib/components/global/CatalogRow.svelte';
	import RegistrationCta from '$lib/components/ui/RegistrationCta.svelte';
	import { getToastState, getCartState, getWishlistState } from '$lib/globals.svelte';
	import { page } from '$app/stores';
	import FormMessageSheet from '../../../../lib/components/ui/FormMessageSheet.svelte';

	let toastState = getToastState();
	let cartState = getCartState();
	let wishlistState = getWishlistState();
	let tabsSlider;
	let tabs;
	let tabsElements;
	let tabsSliderElements;

	let { data, form } = $props();

	
	let formState = $derived(form) 

	function goBack() {
		window.history.back();
	}
	function changeTabs(e) {
		for (let i = 0; i < tabsSliderElements.length; i++) {
			if (e.target === tabsSliderElements[i]) {
				tabsSliderElements.forEach((tab) => {
					tab.classList.remove('text-accent');
					tab.classList.add('border-transparent');
					tab.classList.add('text-textDuller');
					tab.classList.remove('border-accent');
				});
				tabsElements.forEach((tab) => {
					tab.classList.add('hidden');
					tab.classList.remove('flex');
				});
				e.target.classList.add('text-accent');
				e.target.classList.remove('text-textDuller');
				e.target.classList.add('border-accent');
				e.target.classList.remove('border-transparent');
				tabsElements[i].classList.add('flex');
				tabsElements[i].classList.remove('hidden');
			}
		}
	}
	$effect(() => {
		tabsElements = tabs.querySelectorAll(':scope > div');
		tabsSliderElements = tabsSlider.querySelectorAll('button');
		tabsSliderElements[0].click();
	});

	let deliveryContent = {
		title: 'Доставка',
		content: `<div class="flex flex-col gap-4">
								<p>Три способа доставки заказа для розничных клиентов:</p>
							</div>
							<div class="flex flex-col gap-3" id="fromoffice">
								<h3 class="font-serif text-xl">Самовывоз из офиса</h3>
								<p>
									Товары можно забрать по адресу
									<a
										href="https://yandex.ru/maps/213/moscow/house/2_ya_frunzenskaya_ulitsa_10/Z04Ycw9iS0EFQFtvfXtzcnRnYg==/?ll=37.582242%2C55.723846&z=16.9"
									>
										г. Москва, 2-я Фрунзенская 10/1
									</a>
								</p>
							</div>
							<div class="flex flex-col gap-3" id="courier">
								<h3 class="font-serif text-xl">Курьерская доставка</h3>
								<p>
									Доставка собственной курьерской службой по г. Москва (в пределах МКАД) "до двери"-
									400 рублей, при заказе от 3 000 рублей - бесплатно. Оплата наличными.
								</p>
							</div>
							<div class="flex flex-col gap-3" id="cdek">
								<h3 class="font-serif text-xl">Пункты выдачи СДЭК</h3>
								<p>
									Доставка по г. Москва и России осуществляется
									<a href="https://www.cdek.ru/">транспортной компанией СДЭК до пункта ПВЗ</a> (ПВЗ согласовывается
									при оформлении заказа).
								</p>
							</div>
							<div class="flex flex-col gap-3">
								<h3 class="font-serif text-xl">Доставка для оптовых клиентов</h3>
								<p>
									Условия оплаты и доставки уточняйте у вашего менеджера
									<a href="tel:+74956091010"> +7 (495) 609-10-10 </a>.
								</p>
							</div>
`
	};
	let profiContent = {
		title: 'Мастерам и косметологам',
		content: `<p><a href="/register">Зарегестрируйтесь</a> и подтвердите статус мастера в профиле, чтобы получить доступ к специальным ценам на продукцию</p>
		
`
	};
	let contactContent = {
		title: 'Связаться с нами',
	};
	const crosssalesContent = {
		title: 'Вместе берут'
	};
</script>

<section>
	<div class="flex flex-col gap-6 px-5 pt-6 max-md:px-0 max-md:pt-0">
		<div class="max-lg:hidden">
			<Breadcrumbs
				brand={{ name: data.product.brand.name, href: data.product.brand.seo.slug }}
				product={data.product.name}
			/>
		</div>
		<div class="grid grid-cols-[2fr_1fr] gap-16 max-lg:flex max-lg:flex-col max-lg:gap-8">
			<div class="flex h-fit items-center justify-center rounded bg-bgColor max-md:h-svh">
				<div class="gradient absolute left-0 top-0 hidden h-16 w-full max-md:flex"></div>
				<button
					type="button"
					class="absolute left-5 top-7 z-20 hidden h-3 w-3 rotate-180 max-md:flex"
					onclick={goBack}
				>
					<img class="w-full" src="/arrow-navigate.svg" alt="arrow icon" />
				</button>
				<img
					class="w-2/3 object-contain max-md:w-full"
					src={data.product?.thumbnail?.formats?.large
						? CMS_URL + data.product?.thumbnail?.formats?.large?.url
						: CMS_URL + data.product?.thumbnail?.url}
					alt={data.product?.thumbnail?.alternativeText}
					width={data.product?.thumbnail?.width}
					height={data.product?.thumbnail?.height}
				/>
			</div>
			<div class="flex flex-col gap-6 max-lg:items-center max-md:gap-4 max-md:px-5">
				<div class="flex flex-col gap-3 max-lg:items-center max-md:gap-2">
					<h1 class="font-serif text-3xl max-lg:w-2/3 max-lg:text-center max-md:w-full">
						{data.product.name}
					</h1>
					<p class="text-sm text-textDull max-lg:text-center">Артикул: {data.product.SKU}</p>
				</div>
				{#if $page.data?.user?.masterStatus === 'Проверен'}
					<h2 class="flex items-baseline gap-2 text-2xl">
						<span>{data.product.priceRUBOpt} руб</span>
						<span class="text-[#A29C99] line-through opacity-50">{data.product.priceRUB} руб</span>
					</h2>
				{:else}
					<h2 class="text-2xl">{data.product.priceRUB} руб</h2>
				{/if}
				<div
					class="flex w-full items-center gap-0 max-lg:fixed max-lg:bottom-0 max-lg:left-0 max-lg:z-20 max-lg:bg-white max-lg:px-5 max-lg:py-3 max-lg:shadow-[0_-2px_8px_0_rgb(0,0,0,0.1)] max-md:bottom-[60px]"
				>
					<button
						type="button"
						class="btn w-full gap-1"
						onclick={() => {
							toastState.add('Товар добавлен в корзину');
							cartState.add(
								data.product.documentId,
								data.product.brand,
								data.product.name,
								1,
								data.product.priceRUB,
								data.product.priceRUBOpt,
								data.product.thumbnail,
								data.product.shortDesc,
								data.product.seo,
								data.product.SKU
							);
						}}
					>
						<span class="text-white max-md:text-base">Добавить в корзину</span>
						{#if $page.data?.user?.masterStatus === 'Проверен'}
							<span class="hidden text-white max-lg:flex max-md:text-base">
								- {data.product.priceRUBOpt} руб
							</span>
						{:else}
							<span class="hidden text-white max-lg:flex max-md:text-base">
								- {data.product.priceRUB} руб
							</span>
						{/if}
					</button>
					<button
						aria-label="добавить в избранное"
						type="button"
						class:added={wishlistState.products.some(
							(product) => product.documentId === data.product.documentId
						)}
						onclick={(e) => {
							if (e.currentTarget.classList.contains('added')) {
								wishlistState.remove(data.product.documentId);
								e.currentTarget.classList.remove('added');
							} else {
								toastState.add('Товар добавлен в вишлист');
								wishlistState.add(
									data.product.documentId,
									data.product.brand,
									data.product.name,
									1,
									data.product.priceRUB,
									data.product.priceRUBOpt,
									data.product.thumbnail,
									data.product.shortDesc,
									data.product.seo,
									data.product.SKU
								);

								e.currentTarget.classList.add('added');
							}
						}}
						class="flex h-full w-14 items-center justify-center px-5"
					>
						<svg
							class="h-full w-full transition duration-300"
							viewBox="0 0 12 20"
							fill="none"
							xmlns="http://www.w3.org/2000/svg"
						>
							<path
								d="M11.5 0.5H0.5V19.5C2.59224 14.4786 9.40773 14.4786 11.5 19.5V0.5Z"
								stroke="#4A3931"
								stroke-linejoin="round"
							/>
						</svg>
					</button>
				</div>
				<div class="mt-6 flex flex-col gap-6 max-md:mt-4">
					<div bind:this={tabsSlider}>
						<Splide
							aria-label="products"
							options={{
								autoWidth: true,
								perMove: 1,
								snap: true,
								drag: 'free',
								gap: '2rem',
								arrows: false,
								pagination: false,
								breakpoints: {
									1024: {
										perPage: 3
									},
									767: {
										perPage: 2,
										gap: '2rem'
									}
								}
							}}
						>
							{#if data.product.fullDesc}
								<SplideSlide>
									<button
										product-slider
										class="flex border-b pb-4 transition duration-300"
										type="button"
										onclick={changeTabs}>Описание</button
									>
								</SplideSlide>
							{/if}
							{#if data.product.usage}
								<SplideSlide>
									<button
										product-slider
										class="flex border-b pb-4 transition duration-300"
										type="button"
										onclick={changeTabs}>Применение</button
									>
								</SplideSlide>
							{/if}
							{#if data.product.ingredients}
								<SplideSlide>
									<button
										product-slider
										class="flex border-b pb-4 transition duration-300"
										type="button"
										onclick={changeTabs}>Состав</button
									>
								</SplideSlide>
							{/if}
						</Splide>
					</div>
					<div bind:this={tabs}>
						{#if data.product.fullDesc}
							<div class="product-card-desc hidden flex-col gap-3">
								{@html data.product.fullDesc}
							</div>
						{/if}
						{#if data.product.usage}
							<div class="product-card-desc hidden flex-col gap-3">
								{@html data.product.usage}
							</div>
						{/if}
						{#if data.product.ingredients}
							<div class="product-card-desc hidden flex-col gap-3">
								{@html data.product.ingredients}
							</div>
						{/if}
					</div>
				</div>
				<div class="w-full border-t border-borderColor pt-3">
					<SheetContent content={deliveryContent} />
					<SheetContent content={profiContent} />
					<FormMessageSheet content={contactContent} {formState} {data} action={'?/sendMessage'} />
				</div>
			</div>
		</div>
	</div>
	<div class="splide__list hidden"></div>
</section>
{#key data}
	<CatalogRow
		content={crosssalesContent}
		products={data.crosssales.filter((product) => product.documentId !== data.product.documentId)}
	/>
{/key}
<RegistrationCta />

<style>
	:global(.splide__list:has(button[product-slider])) {
		border-bottom: 1px solid #ebe6e1;
	}
	.gradient {
		background: rgb(0, 0, 0);
		background: -moz-linear-gradient(180deg, rgba(0, 0, 0, 0.2) 0%, rgba(0, 0, 0, 0) 100%);
		background: -webkit-linear-gradient(180deg, rgba(0, 0, 0, 0.2) 0%, rgba(0, 0, 0, 0) 100%);
		background: linear-gradient(180deg, rgba(0, 0, 0, 0.2) 0%, rgba(0, 0, 0, 0) 100%);
		filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#000000",endColorstr="#000000",GradientType=1);
	}
	:global(.product-card-desc iframe) {
		width: 100% !important;
		height: 100% !important;
		aspect-ratio: 16/9 !important;
	}
	.added svg {
		fill: #4a3931;
	}
</style>
