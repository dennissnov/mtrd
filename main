// Global variables
const preloader = document.querySelector('.preloader');
const preloaderElement = document.querySelector('.preloader__element');
const preloaderTL = gsap.timeline();
// Create SplitType elements
const heroTitle = new SplitType('.hero__heading', { types: 'chars' });
const heroKicker = new SplitType('.kicker--hero', { types: 'chars' });
const sectionTitle = new SplitType('.section-title', { types: 'chars' });
const introText = new SplitType('.intro-text', { types: 'words' });
var aboutPara = gsap.timeline(),
	mySplitText = new SplitText('#about-para', { type: 'words,chars' }),
	chars = mySplitText.chars;
gsap.set('#about-para', { perspective: 400 });
// Preloader + Intro Animation

if (preloaderElement) {
	preloaderTL
		.from(preloaderElement, {
			y: 150,
			duration: 1.75,
			ease: 'power4.inOut',
		})
		.to(
			preloaderElement,
			{
				opacity: 1,
				duration: 2,
				ease: 'power4.inOut',
			},
			'<'
		)
		.to('.preloader > *', {
			yPercent: -100,
			stagger: {
				amount: 0.4,
				from: 'random',
			},
			duration: 0.85,
			ease: 'power4.inOut',
		})
		.from(
			heroTitle.chars,
			{
				opacity: 0,
				filter: 'blur(60px)',
				y: 50,
				duration: 0.75,
				stagger: 0.075,
				ease: 'sine.out',
			},
			'<1.25'
		)
		.from(
			'.hero__deco',
			{
				y: 50,
				filter: 'blur(30px)',
				opacity: 0,
				stagger: 0.75,
				duration: 3,
				ease: 'power4.inOut',
			},
			'-=5'
		);
}

const flipAnimation2 = (main) => {
	console.log('asu');
	const imageWidth = '100vw';
	const fullImageHeight = '100vh';
	const halfImageHeight = '20vh';
	let heightDiff, midY;
	let flipAnim;
	const mousePosition = new Vec2(0, 0);
	const curtains = new Curtains({
		container: 'canvas',
		autoRender: false,
		depth: false,
	});
	gsap.ticker.add(() => curtains.render());
	const planes = [];
	const params = {
		vertexShaderID: 'slider-planes-vs',
		fragmentShaderID: 'slider-planes-fs',
		widthSegments: 20,
		heightSegments: 20,
		uniforms: {
			time: {
				name: 'uTime',
				type: '1f',
				value: 0,
			},
			mousePosition: {
				name: 'uMousePosition',
				type: '2f',
				value: mousePosition,
			},
			mouseStrength: {
				name: 'uMouseStrength',
				type: '1f',
				value: 0,
			},
			rippleFactor: {
				name: 'uRippleFactor',
				type: '1f',
				value: 2.5,
			},
		},
	};

	const article = main.querySelector('article');
	gsap.utils.toArray('article').forEach((article, i) => {
		const planeContainer = article.querySelector('.plane-container');
		const planeElem = planeContainer.querySelector('.plane');
		const placeholderImage = planeElem.querySelector('img');

		const fullSizeImage = document.createElement('img');
		const canvasId = document.getElementById('canvas');
		fullSizeImage.crossorigin = 'anonymous';
		fullSizeImage.dataset.sampler = 'planeTexture';
		fullSizeImage.src = '../images/ASRV---Tom-Joy--IMG_8362-.jpg';

		let isOpen = false;

		fullSizeImage.addEventListener(
			'load',
			() => {
				planeElem.replaceChild(fullSizeImage, placeholderImage);

				const plane = new Plane(curtains, planeElem, params);

				if (plane) {
					plane.setPerspective(35);
					planes.push(plane);

					plane
						.onReady(function () {
							fullSizeImage.classList.add('hidden');
						})
						.onRender(function () {
							plane.uniforms.time.value++;
						});
				}
				setTimeout(() => {
					if (flipAnim && flipAnim.isActive()) return;

					fullSizeImage.classList.add('hidden');
					plane.setRenderOrder(1);
					gsap.set(planeElem, { zIndex: 1 });

					if (!isOpen) {
						const state = Flip.getState(planeElem);

						planeContainer.classList.add('open');
						gsap.set(planeElem, {
							top: '0%',
							yPercent: -20,
							clipPath: 'inset(20% 20% 0% 0%)',
						});
						// clippath canvas
						// planeElem.style.clipPath = 'inset(20% 20% 0% 0%)';
						gsap.to(canvasId, { clipPath: 'inset(0% 0% 30% 0%)' });

						flipAnim = Flip.from(state, {
							duration: 1.5,
							onUpdate: updatePlanes,
							ease: 'power1.inOut',
							onComplete: () => {
								// Setup translation of image
							},
						});

						plane.uniforms.rippleFactor.value = 2.5;
					} else {
						const state = Flip.getState(planeElem);

						planeContainer.classList.remove('open');
						gsap.set(planeElem, {
							top: 0,
							y: 0,
							yPercent: 0,
							scaleY: 1,
						});
						gsap.to(canvasId, { clipPath: 'inset(0% 0% 0% 0%)' });

						flipAnim = Flip.from(state, {
							onUpdate: updatePlanes,
							onComplete: () => {
								gsap.set(planeElem, { zIndex: 0 });
								plane.setRenderOrder(0);
							},
						});

						plane.uniforms.rippleFactor.value = 3;
					}

					animateWaves(plane);

					isOpen = !isOpen;
				}, 300);

				planeElem.addEventListener('click', (e) => {
					if (flipAnim && flipAnim.isActive()) return;

					fullSizeImage.classList.add('hidden');
					plane.setRenderOrder(1);
					gsap.set(planeElem, { zIndex: 1 });

					if (!isOpen) {
						const state = Flip.getState(planeElem);

						planeContainer.classList.add('open');
						gsap.set(planeElem, {
							top: '0%',
							yPercent: -20,
							clipPath: 'inset(20% 20% 0% 0%)',
						});
						// clippath canvas
						// planeElem.style.clipPath = 'inset(20% 20% 0% 0%)';
						gsap.to(canvasId, { clipPath: 'inset(0% 0% 30% 0%)' });

						flipAnim = Flip.from(state, {
							duration: 1.5,
							onUpdate: updatePlanes,
							ease: 'power1.inOut',
							onComplete: () => {
								// Setup translation of image
							},
						});

						plane.uniforms.rippleFactor.value = 2.5;
					} else {
						const state = Flip.getState(planeElem);

						planeContainer.classList.remove('open');
						gsap.set(planeElem, {
							top: 0,
							y: 0,
							yPercent: 0,
							scaleY: 1,
						});
						gsap.to(canvasId, { clipPath: 'inset(0% 0% 0% 0%)' });

						flipAnim = Flip.from(state, {
							onUpdate: updatePlanes,
							onComplete: () => {
								gsap.set(planeElem, { zIndex: 0 });
								plane.setRenderOrder(0);
							},
						});

						plane.uniforms.rippleFactor.value = 3;
					}

					animateWaves(plane);

					isOpen = !isOpen;
				});
			},
			{ once: true }
		);
	});

	function animateWaves(plane) {
		// if (e.targetTouches) {
		// 	mousePosition.x = e.targetTouches[0].clientX;
		// 	mousePosition.y = e.targetTouches[0].clientY;
		// } else {
		// 	mousePosition.x = e.clientX;
		// 	mousePosition.y = e.clientY;
		// }

		// create dummy coordinate randomly
		const x = Math.random() * window.innerWidth;
		const y = Math.random() * window.innerHeight;
		mousePosition.x = x;
		mousePosition.y = y;

		const mouseCoords = plane.mouseToPlaneCoords(mousePosition);
		plane.uniforms.mousePosition.value = mouseCoords;

		gsap.to(plane.uniforms.mouseStrength, {
			value: 1.7,
			duration: flipAnim.duration(),
			ease: CustomEase.create(
				'custom',
				'M0,0,C0,0,0.077,1,0.244,1,0.316,1,0.447,0.525,0.514,0.442,0.668,0.248,1,0,1,0'
			),
		});
	}

	function updatePlanes() {
		planes.forEach((plane) => plane.updatePosition());
	}

	// function handleResize() {
	// 	heightDiff = (imageHeight - innerHeight) / 2;
	// 	midY = innerHeight / 2;

	// 	updatePlanes();
	// }

	// window.addEventListener('resize', handleResize);
	// handleResize();
};

const initBarba = () => {
	barba.hooks.beforeEnter((data) => {
		// this.identifierPage(data);
		// this.initAlways(data);
	});

	barba.hooks.beforeLeave((data) => {
		// this.destroyAlways(data);
		// this.toggleScrollOpacity(0);
		ScrollTrigger.getAll().forEach((trigger) => {
			trigger.kill();
		});
	});

	barba.hooks.once((data) => {
		// this.initOnce(data);
	});

	barba.hooks.afterEnter((data) => {
		//this.recalculateSmoothScroll();
		// this.initAlwaysEnter(data);
		// this.toggleScrollOpacity(1, 500);
		console.log(data, 'data after enter');
		window.scrollTo(0, 0);
		flipAnimation2(data.next.container);

		// const planeElement = document.querySelector('.plane');
		// if (planeElement) {
		// 	planeElement.click(); // Simulate a click on the planeElement
		// }
	});

	barba.hooks.leave((data) => {
		// setTimeout(() => {
		// 	this.mm.add('(min-width:992px)', () => {
		// 		window.scroller.scrollTo(
		// 			document.querySelector('[data-scroll-container]').offsetTop,
		// 			{
		// 				duration: 0.1, //0.1
		// 				disableLerp: true,
		// 			}
		// 		);
		// 	});
		// 	this.mm.add('(max-width:991px)', () => {
		// 		window.scrollTo(0, 0);
		// 	});
		// }, this.speed * 1000);
		// window.scrollTo(0, 0);
	});

	const flipAnimation = (main) => {
		const container1 = main.querySelector('.image-target'),
			container2 = main.querySelector('.image-source'),
			box = main.querySelector('.img');

		console.log(container1, container2, box, Flip, 'flip');

		if (Flip) {
			const state = Flip.getState(box);
			if (!container1 || !container2 || !box) return;

			if (box.parentElement === container1) {
				container2.appendChild(box);
			} else {
				container1.appendChild(box);
			}

			Flip.from(state, {
				duration: 1,
				ease: 'expo.out',
			});
		}
	};

	barba.init({
		debug: true,
		timeout: 12000,
		sync: true,
		transitions: [
			{
				name: 'self',
				once: ({ next }) => {},
				leave(data) {
					// return gsap.to(data.current.container, {
					// 	opacity: 0,
					// 	duration: 1,
					// });
				},
				enter(data) {
					// gsap.set(data.next.container, { autoAlpha: 1 });
					data.next.container;

					console.log('asuuuuuu');

					const planeElement =
						data.next.container.querySelector('.plane');
					console.log(planeElement);

					if (planeElement) {
						planeElement.click(); // Simulate a click on the planeElement
					}
					// flipAnimation(data.next.container);

					// flipAnimation(data.next.container);
					// this.enterTransititon = gsap
					// 	.timeline()
					// 	.from(data.next.container, {
					// 		autoAlpha: 0,
					// 		ease: 'circ.out',
					// 		delay: 0.5,
					//         scale:0.9,
					// 		y: 200,
					// 		duration: 1,
					// 		clearProps: 'y',
					// 		onStart: () => {},
					// 	});
				},
			},
		],
	});
	window.barba = barba;
};

document.addEventListener('DOMContentLoaded', function () {
	gsap.registerPlugin(
		ScrollTrigger,
		SplitText,
		SplitType,
		ScrambleTextPlugin,
		Flip
	);

	// Initialize SplitText for .h2-random elements ahead of Swiper to avoid race conditions
	const h2RandomElements = document.querySelectorAll('.h2-random');
	const splitTexts = {}; // Store SplitText instances keyed by element ID or index
	if (h2RandomElements.length > 0) {
		h2RandomElements.forEach((el, index) => {
			// Ensure each .h2-random element has an ID for reference
			const id = el.id || `h2-random-${index}`;
			el.id = id; // Assign ID if not present
			// Initialize SplitText and store the instance
			splitTexts[id] = new SplitText(el, { type: 'chars' });
		});
	}

	// Function to animate .h2-random elements, called on slide change
	// function animateH2Random(id) {
	// 	const splitText = splitTexts[id];
	// 	if (splitText && splitText.chars) {
	// 		gsap.fromTo(
	// 			splitText.chars,
	// 			{
	// 				opacity: 0,
	// 				y: 20,
	// 				visibility: 'hidden',
	// 			},
	// 			{
	// 				opacity: 1,
	// 				y: 0,
	// 				visibility: 'visible',
	// 				duration: 0.5,
	// 				stagger: 0.05,
	// 				ease: 'power2.out',
	// 				overwrite: 'auto',
	// 			}
	// 		);
	// 	}
	// }

	// Function to set up and animate split text elements
	function setupSplitTextAnimations() {
		// Hero Title SplitText and Animation
		if (!document.querySelector('.hero__heading')) return;
		const heroTitle = new SplitText('.hero__heading', {
			type: 'chars, words',
		});

		gsap.from(heroTitle.chars, {
			opacity: 0,
			scale: 0,
			transformOrigin: 'center',
			duration: 0.5,
			stagger: 0.1,
			ease: 'back.out(1.7)',
			scrollTrigger: {
				trigger: '.hero__heading',
				start: 'top 90%',
				toggleActions: 'restart pause resume pause',
				scroller: '#container',
			},
		});
		// Kicker SplitText and Animation
		const heroKicker = new SplitText('.kicker--hero', {
			type: 'chars, words',
		});
		gsap.from(heroKicker.chars, {
			opacity: 0,
			y: 50,
			duration: 0.5,
			stagger: 0.05,
			ease: 'power2.out',
			scrollTrigger: {
				trigger: '.kicker--hero',
				start: 'top 90%',
				toggleActions: 'restart pause resume pause',
				scroller: '#container',
			},
		});
		gsap.to('#hero-scramble-line-1', {
			duration: 4.8,
			scrambleText: {
				text: 'Mattered is an award-winning',
				chars: '⠀⠁⠂⠃⠄⠅⠆⠇⠈⠉⠊⠋⠌⠍⠎⠏⠐⠑⠒⠓⠔⠕⠖⠗⠘⠙⠚⠛⠜⠝⠞⠟⠠⠡⠢⠣⠤⠥⠦⠧⠨⠩⠪⠫⠬⠭⠮⠯⠰⠱⠲⠳⠴⠵⠶⠷⠸⠹⠺⠻⠼⠽⠾⠿',
				revealDelay: 4.2,
				tweenLength: false,
				scrollTrigger: {
					trigger: '.hero__heading',
					start: 'top 90%',
					toggleActions: 'restart pause resume pause',
					scroller: '#container',
				},
			},
		});
		// Section Title SplitText and Animation
		gsap.to('#hero-scramble-line-2', {
			duration: 5,
			scrambleText: {
				text: 'agency based in Irvine, CA.',
				revealDelay: 4.4,
				chars: '⠀⠁⠂⠃⠄⠅⠆⠇⠈⠉⠊⠋⠌⠍⠎⠏⠐⠑⠒⠓⠔⠕⠖⠗⠘⠙⠚⠛⠜⠝⠞⠟⠠⠡⠢⠣⠤⠥⠦⠧⠨⠩⠪⠫⠬⠭⠮⠯⠰⠱⠲⠳⠴⠵⠶⠷⠸⠹⠺⠻⠼⠽⠾⠿',
				tweenLength: false,
				scrollTrigger: {
					trigger: '.hero__heading',
					start: 'top 90%',
					toggleActions: 'restart pause resume pause',
					scroller: '#container',
				},
			},
		});
		// Additional Elements Here
		// Implement animations for `.text-accent`, `.text-scramble`, etc., following the above pattern
	}
	setupSplitTextAnimations();

	// SLIDES
	const allSlides = document.querySelectorAll('.section');

	if (allSlides.length > 0) {
		allSlides.forEach(function (slide, index) {
			const image = slide.querySelector('.image-desktop');
			const tl = gsap.timeline({ paused: true,delay:0.5 });

			tl.from(image, {
				clipPath: 'inset(100% 0 0 0)',
				duration: 2,
				ease: 'power4.out',
			});

			this.activeIndex = index;
			ScrollTrigger.create({
				trigger: slide,
				start: 'top bottom',
				end: 'bottom bottom',
				markers: true,
				scroller: '#container',
				onEnter: function () {
					// const initialH2Random = slide.querySelector('.h2-random');
					// if (initialH2Random) {
					// 	animateH2Random(index);
					// }

					slide.classList.add('active');
					tl.play();
					const h2Random = slide.querySelector('.h2-random');
					if (h2Random) {
						gsap.timeline()
							.set(h2Random, { autoAlpha: 1, overflow: 'hidden' })
							.fromTo(
								h2Random.querySelectorAll('div'),
								{
									yPercent: 100,
									autoAlpha: 0,
								},
								{
									yPercent: 0,
									duration: 1,
									autoAlpha: 1,
									stagger: 0.05,
									ease: 'power4.out',
								}
							);
					}

					// Specific animations for slides, like scramble text
					if (this.activeIndex === 1) {
						// Adjust index as per your setup
						gsap.to('#scramble', {
							duration: 1.5,
							scrambleText: {
								text: 'What we offer as a team',
								chars: 'lowerCase',
								revealDelay: 0.5,
								tweenLength: false,
							},
						});
					}
					// Specific animations for slides, like scramble text
					if (this.activeIndex === 1) {
						// Adjust index as per your setup
						gsap.from('.client-square-logo', {
							y: 0,
							filter: 'blur(30px)',
							delay: 1,
							stagger: 0.0,
							duration: 1,
							ease: 'power4.inOut',
						});
						gsap.to('.client-square-logo', {
							opacity: 1,
						});
					}
					if (this.activeIndex === 1) {
						// slide = 2, so index = 1
						// About para SplitText and Animation
						aboutPara.from(chars, {
							duration: 1,
							scale: 0,
							y: 60,
							ease: 'power4.out',
							filter: 'blur(60px)',
							stagger: 0.002,
						});
						gsap.to('.about-para', {
							opacity: 1,
						});
					}
					if (this.activeIndex === 0) {
						// Adjust index as per your setup
						gsap.to('#hero-scramble-line-1', {
							duration: 1.5,
							scrambleText: {
								text: 'Mattered is an award-winning',
								chars: '⠀⠁⠂⠃⠄⠅⠆⠇⠈⠉⠊⠋⠌⠍⠎⠏⠐⠑⠒⠓⠔⠕⠖⠗⠘⠙⠚⠛⠜⠝⠞⠟⠠⠡⠢⠣⠤⠥⠦⠧⠨⠩⠪⠫⠬⠭⠮⠯⠰⠱⠲⠳⠴⠵⠶⠷⠸⠹⠺⠻⠼⠽⠾⠿',
								revealDelay: 0.5,
								tweenLength: false,
							},
						});
					}
					if (this.activeIndex === 0) {
						// Adjust index as per your setup
						gsap.to('#hero-scramble-line-2', {
							duration: 1.5,
							scrambleText: {
								text: 'agency based in Irvine, CA.',
								revealDelay: 0.5,
								chars: '⠀⠁⠂⠃⠄⠅⠆⠇⠈⠉⠊⠋⠌⠍⠎⠏⠐⠑⠒⠓⠔⠕⠖⠗⠘⠙⠚⠛⠜⠝⠞⠟⠠⠡⠢⠣⠤⠥⠦⠧⠨⠩⠪⠫⠬⠭⠮⠯⠰⠱⠲⠳⠴⠵⠶⠷⠸⠹⠺⠻⠼⠽⠾⠿',
								tweenLength: false,
							},
						});
					}
				},
				onLeaveBack: function () {
					slide.classList.remove('active');
					tl.reverse();
				},
			});
		});
	}


	initBarba();
	const flipAnimation = (main) => {
		const container1 = main.querySelector('.image-target'),
			container2 = main.querySelector('.image-source'),
			box = main.querySelector('.img');

		console.log(container1, container2, box, Flip, 'flip');

		if (Flip) {
			const state = Flip.getState(box);
			if (!container1 || !container2 || !box) return;

			if (box.parentElement === container1) {
				container2.appendChild(box);
			} else {
				container1.appendChild(box);
			}

			Flip.from(state, {
				duration: 10,
				ease: 'expo.out',
			});
		}
	};
	console.log(document.querySelector('main'));
	// flipAnimation(document.querySelector('main'));
});
