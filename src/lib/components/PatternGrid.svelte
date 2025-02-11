<script lang="ts">
	import KnittingSymbol from './KnittingSymbol.svelte';

	export let mode: string;
	export let nPanel: number;
	export let refreshKey: number;
	export let increases: string[];
	export let decreases: string[];

	// In PatternGrid.svelte
	export let canvas: HTMLCanvasElement;
	// export let refreshKey = 0;
	import { onMount } from 'svelte';
	// Grid dimensions
	let rows = Math.random() < 0.5 ? 6 : 8;
	let panelSize = nPanel === 4 ? 25 : nPanel === 5 ? 20 : nPanel === 6 ? 17 : 0;
	let cols = panelSize;
	let numIncDecPairs = Math.floor(Math.random() * (Math.floor((panelSize - 6) / 2) - 3 + 1)) + 3;
	let patternArrray: string[] = [];
	let knitArray: string[] = [];
	let patternRows: string[][] = Array.from({ length: Math.round(rows / 2) }, () => []);
	function fillPatternArray() {
		// console.log(mode);
		if (mode === 'lacy') {
			for (let i = 0; i < numIncDecPairs; i++) {
				patternArrray.push(randomIncrease());
				patternArrray.push(randomDecrease());
			}
			while (patternArrray.length < panelSize) {
				patternArrray.push('knit');
			}
		} else {
			for (let i = 0; i < panelSize; i++) {
				patternArrray.push(randomKP());
			}
		}
	}
	function shuffleArray(array: string[]): string[] {
		const copy = [...array]; // Make a shallow copy
		for (let i = copy.length - 1; i > 0; i--) {
			const j = Math.floor(Math.random() * (i + 1)); // Random index
			[copy[i], copy[j]] = [copy[j], copy[i]]; // Swap elements
		}
		return copy;
	}
	function randomKP() {
		return Math.random() < 0.5 ? 'knit' : 'purl';
	}
	function randomIncrease() {
		const randomIndex = Math.floor(Math.random() * increases.length);
		return increases[randomIndex];
	}
	function randomDecrease() {
		const randomIndex = Math.floor(Math.random() * decreases.length);
		return decreases[randomIndex];
	}
	function getStitch(row: number, col: number) {
		return patternRows[row][col];
	}

	function drawCanvas() {
		if (!canvas) return;
		const ctx = canvas.getContext('2d', { alpha: false });
		ctx.imageSmoothingEnabled = true;
		ctx.imageSmoothingQuality = 'high';

		const scale = window.devicePixelRatio || 1;
		canvas.width = 816 * scale;
		canvas.height = 1056 * scale;
		ctx.scale(scale, scale);

		// Background
		ctx.fillStyle = 'white';
		ctx.fillRect(0, 0, canvas.width, canvas.height);
		let y = 50;
		//image
		const img = new Image();
		img.src = '/Warmwyrm.png';
		img.onload = () => {
			ctx.drawImage(img, 50, 0, 170, 170);
		};
		// Add hat image to the right middle of the page
		const hatImg = new Image();
		hatImg.src = '/hat.png';
		hatImg.onload = () => {
			const hatX = 540;
			const hatY = 400;
			ctx.drawImage(hatImg, hatX, hatY, 180, 180);
		};
		// Heading
		ctx.font = 'bold 30px Arial';
		ctx.fillStyle = 'black';
		ctx.textAlign = 'center';
		ctx.fillText('Wyrm Bean', 408, y);

		y += 50;

		// Heading
		ctx.font = 'bold 18px Arial';
		ctx.fillText('Algorithmic beanies for the modern dragon.', 408, y);
		y += 50;

		// description
		ctx.font = '16px Arial';
		const text = `This medium/small hat knitted from bottom to top. The hat has a pattern that repeats ${nPanel || 5} times each round. Size can be adjusted with yarn/needles and the number of times Chart A is worked.`;
		const maxWidth = 550;
		const lineHeight = 20;
		const words = text.split(' ');
		let line = '';
		let testLine = '';
		let testWidth = 0;
		let lineArray = [];

		for (let n = 0; n < words.length; n++) {
			testLine = line + words[n] + ' ';
			testWidth = ctx.measureText(testLine).width;
			if (testWidth > maxWidth && n > 0) {
				lineArray.push(line);
				line = words[n] + ' ';
			} else {
				line = testLine;
			}
		}
		lineArray.push(line);

		for (let k = 0; k < lineArray.length; k++) {
			ctx.fillText(lineArray[k], 408, y);
			y += lineHeight;
		}

		y += 20;

		// Supplies
		ctx.fillStyle = 'black';
		ctx.font = 'bold 16px Arial';
		ctx?.fillText('Supplies', 650, y + 20);
		ctx.font = '16px Arial';
		ctx.textAlign = 'center';
		const suppliesText = [
			' ',
			'~230yds of worsted weight yarn,',
			'3.5mm and 4mm dpn or circular,',
			'markers, embroidery needle,',
			'the determination of a dragon'
		];
		const boxX = 500;
		const boxY = y;
		const boxWidth = 300;
		const boxHeight = suppliesText.length * 20 + 20;

		// Draw box
		ctx.strokeStyle = 'black';
		ctx.lineWidth = 1;
		ctx.strokeRect(boxX, boxY, boxWidth, boxHeight);

		// Draw text inside the box
		suppliesText.forEach((line, index) => {
			ctx.fillText(line, boxX + 150, boxY + 25 + index * 20);
		});
		y += 10;
		//I need to add a legend for the symbols here knit, purl, ssk, k2tog, yo, m1r, m1l, s2kp2 in a 2x4 grid
		// legend
		// Legend
		const legendX = 550;
		const legendY = y + 550;
		const legendWidth = 200;
		const legendHeight = 180;
		const legendGridSize = 20;
		const legendItems = [
			{ type: 'knit', label: 'knit' },
			{ type: 'purl', label: 'purl' },
			{ type: 'yo', label: 'yo' },
			{ type: 'ssk', label: 'ssk' },
			{ type: 'k2tog', label: 'k2tog' },
			{ type: 'm1l', label: 'm1l' },
			{ type: 'm1r', label: 'm1r' },
			{ type: 's2kp2', label: 's2kp2' }
		];

		// Draw legend box
		ctx.fillStyle = 'black';
		ctx.font = 'bold 16px Arial';
		ctx?.fillText('Legend', 650, y + 570);
		ctx.strokeStyle = 'black';
		ctx.lineWidth = 1;
		ctx.strokeRect(legendX, legendY, legendWidth, legendHeight);

		// Draw legend items
		legendItems.forEach((item, index) => {
			const col = index % 2;
			const row = Math.floor(index / 2);
			const x = 30 + legendX + col * (legendGridSize * 4);
			const y = 30 + legendY + row * (legendGridSize * 2);

			renderSymbol(ctx, item.type, x, y, legendGridSize);

			ctx.font = '16px Arial';
			ctx.fillStyle = 'black';
			ctx.textAlign = 'left';
			ctx.fillText(item.label, x + legendGridSize + 5, y + legendGridSize / 2 + 4);
		});

		// Title
		ctx.font = 'bold 24px Arial';
		ctx.textAlign = 'left';
		ctx.fillStyle = 'black';
		ctx.fillText('The Pattern', 50, y);
		y += 50;
		// Instructions
		ctx.font = '16px Arial';
		let nStsAround = nPanel * panelSize;
		const steps = [
			`Cast on ${Math.round(panelSize * nPanel) || 100} stitches to 3.5mm circular needles.`,
			'Repeat {k,p} until there is 1.5in of ribbing.',
			'For the rest of the hat switch to 4mm needles.',
			'Work Chart A 5-7 times based on desired height. Place markers. ',
			'Work Chart B 1 time (switch to dpns when needed).',
			'To close, repeat {ssk,k,k} until 6-8 stitches remain.',
			'Cut yarn and thread tail through embroidery needle.',
			'Pass needle through the remaining stitches and cinch.',
			'Weave in the ends.'
		];

		steps.forEach((step, i) => {
			ctx.fillText(`${i + 1}. ${step}`, 50, y);
			y += 25;
		});

		// Chart A title
		y += 20;
		ctx.font = 'bold 18px Arial';
		ctx.fillText('Chart A', 50, y);

		// Draw grid pattern
		const gridSize = 20;
		const startX = 50;
		const startY = y + 25;

		for (let row = 0; row < rows; row++) {
			for (let col = 0; col < cols; col++) {
				const x = startX + col * gridSize;
				const y = startY + row * gridSize;
				const stitch = getStitch(row, col);
				renderSymbol(ctx, stitch, x, y, gridSize);
			}
		}
		y += 215;
		ctx.fillStyle = 'black';
		ctx.font = 'bold 18px Arial';
		ctx.fillText('Chart B', 50, y);
		// Add the closing pattern code here
		const bStartY = y + 20;
		function generateClosingRows() {
			let closingRows = [];
			let nStitches = panelSize;
			let nDepth = nPanel === 4 ? 16 : nPanel === 5 ? 9 : nPanel === 6 ? 6 : 0;

			while (nStitches >= 6) {
				let knitRow = Array(nStitches).fill('knit');
				closingRows.push(knitRow);
				nStitches -= 2;
				if (nStitches < nDepth) {
					nStitches -= 2;
				}
				let decRow = [];
				if (nStitches > nDepth) {
					if (mode === 'lacy') {
						decRow.push(randomIncrease(), randomDecrease(), 's2kp2');
					} else {
						decRow.push('purl', 'purl', 's2kp2');
					}
					for (let i = 3; i < nStitches; i++) {
						decRow.push('knit');
					}
				} else {
					decRow.push('s2kp2', 's2kp2');
					for (let i = 2; i < nStitches; i++) {
						decRow.push('knit');
					}
				}
				closingRows.push(shuffleArray(decRow));
			}
			return closingRows;
		}

		const closingRows = generateClosingRows();
		for (let i = closingRows.length - 1; i >= 0; i--) {
			const row = closingRows[i];
			for (let j = 0; j < row.length; j++) {
				renderSymbol(
					ctx,
					row[j],
					50 + j * gridSize,
					bStartY + (closingRows.length - 1 - i) * gridSize,
					gridSize
				);
			}
		}
	}
	onMount(drawCanvas);
	//runes  4 of them state/props/variables/effect
	$: {
		if (mode || nPanel || increases || decreases || refreshKey) {
			panelSize = nPanel === 4 ? 25 : nPanel === 5 ? 20 : nPanel === 6 ? 17 : 20;
			cols = panelSize;
			patternArrray = [];
			fillPatternArray();
			knitArray = ['knit', ...Array(panelSize - 1).fill('knit')];
			for (let i = 0; i < rows; i++) {
				if (i % 2 === 1) {
					// Create a new array copy for even rows
					patternRows[i] = [...knitArray];
				} else {
					// Shuffle a new copy of the pattern array for odd rows
					patternRows[i] = shuffleArray([...patternArrray]);
				}
			}
		}
		drawCanvas();
	}

	function renderSymbol(ctx, type, x, y, size) {
		// Square border for all symbols
		ctx.strokeStyle = 'black';
		ctx.lineWidth = 2;
		ctx.strokeRect(x, y, size, size);
		ctx.fillStyle = 'white';
		ctx.fillRect(x, y, size, size);

		if (type === 'purl') {
			ctx.beginPath();
			ctx.arc(x + size / 2, y + size / 2, 4, 0, Math.PI * 2);
			ctx.fillStyle = 'black';
			ctx.fill();
		} else if (type === 'yo') {
			ctx.beginPath();
			ctx.arc(x + size / 2, y + size / 2, 4, 0, Math.PI * 2);
			ctx.stroke();
		} else {
			ctx.beginPath();
			switch (type) {
				case 'k2tog':
					ctx.moveTo(x + 17, y + 3);
					ctx.lineTo(x + 3, y + 17);
					break;
				case 'ssk':
					ctx.moveTo(x + 3, y + 3);
					ctx.lineTo(x + 17, y + 17);
					break;
				case 'm1l':
					ctx.moveTo(x + 5, y + 5);
					ctx.lineTo(x + 15, y + 15);
					ctx.moveTo(x + 10, y + 10);
					ctx.lineTo(x + 15, y + 5);
					break;
				case 'm1r':
					ctx.moveTo(x + 15, y + 5);
					ctx.lineTo(x + 5, y + 15);
					ctx.moveTo(x + 10, y + 10);
					ctx.lineTo(x + 5, y + 5);
					break;
				case 's2kp2':
					ctx.moveTo(x + 10, y + 5);
					ctx.lineTo(x + 10, y + 15);
					ctx.moveTo(x + 10, y + 5);
					ctx.lineTo(x + 15, y + 15);
					ctx.moveTo(x + 10, y + 5);
					ctx.lineTo(x + 5, y + 15);
					break;
			}
			ctx.stroke();
		}
	}
</script>

<canvas
	bind:this={canvas}
	width={816}
	height={1056}
	class="mt-4 w-full max-w-2xl border border-gray-100"
	style="aspect-ratio: 8.5/11"
></canvas>
