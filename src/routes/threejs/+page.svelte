<script>
</script>

<svelte:head>
	<title>Threejs</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<h1>THREEJS EXAMPLE</h1>
	<ul>
		<li>
			<a href='/threejs/custom-model-load'>커스텀 모델 로드 (custom-model-load)</a>
		</li>
        <li>
			<a href='/threejs/custom-model-animation'>커스텀 모델 애니메이션 (custom-model-animation)</a>
		</li>
        <li>
			<a href='/threejs/physics-engine-world'>물리엔진 월드 생성 (physics-engine-world)</a>
		</li>
        <li>
			<a href='/threejs/contact-material'>재질에 다른 마찰력과 반발력 (contact-material)</a>
		</li>
        <li>
			<a href='/threejs/force'>힘 (Force) - 화면을 클릭</a>
		</li>
	</ul>
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	h1 {
		width: 100%;
	}
</style>
