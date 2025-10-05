<script lang="ts">
    import { onMount } from 'svelte';

    const flavorText: string[] = [
        "Reticulating spines",
        "Generating witty dialog",
        "Optimizing the optimizer",
        "Re-linting the source code",
        "Un-linting the source code",
        "Pinging 127.0.0.1",
        "Generating random flavor text",
        "Recursing recursively",
        "Passing the Turing test",
        "Failing unit tests",
        "Consulting with legal",
        "Moving away from the mic to breathe",
        "Manually rewriting commit history",
        "Downloading more RAM",
        "Finding the \"Any\" key",
        "Waiting at the coffee machine",
        "Turning it off and on again"
    ];

    let displayedLines: string[] = [];
    let currentText: string = '';
    let currentDots: string = '';
    let usedIndices: Set<number> = new Set();

    const getRandomUnusedText = () => {
        if (usedIndices.size === flavorText.length) {
            usedIndices.clear();
        }
        
        let randomIndex;
        do {
            randomIndex = Math.floor(Math.random() * flavorText.length);
        } while (usedIndices.has(randomIndex));

        usedIndices.add(randomIndex);
        return flavorText[randomIndex];
    };

    const typeText = (text: string): Promise<void> => {
        currentText = '';
        currentDots = '';

        return new Promise((resolve) => {
            let charIndex = 0;
            const typingInterval = setInterval(() => {
                if (charIndex < text.length) {
                    currentText += text[charIndex];
                    charIndex++;
                } else {
                    clearInterval(typingInterval);
                    resolve();
                }
            }, 45); // typing speed
        });
    };

    const addDots = (): Promise<void> => {
        return new Promise((resolve) => {
            let dotCount = 0;
            
            const dotInterval = setInterval(() => {
                if (dotCount < 3) {
                    currentDots += '.';
                    dotCount++;
                } else {
                    clearInterval(dotInterval);
                    resolve();
                }
            }, 350); // 350ms per dot
        });
    };

    async function animateNextLine() {
        const nextText = getRandomUnusedText();
        
        if (displayedLines.length >= 3) {
            displayedLines = displayedLines.slice(1);
        }
        
        await typeText(nextText);
        
        await addDots();
        
        displayedLines = [...displayedLines, nextText + currentDots];
        
        currentText = '';
        currentDots = '';
        
        setTimeout(() => {
            animateNextLine();
        }, 300);
    }

    onMount(() => {
        animateNextLine();
    });
</script>

<div class="bg-gray-800 text-amber-200 px-8 py-4 rounded-md font-mono h- overflow-hidden text-[0.875rem] md:text-[1rem]">
    <code class="h-full flex flex-col justify-start">
        <!-- Display completed lines -->
        {#each displayedLines as line}
            <div class="transition-all duration-300 leading-6">{line}</div>
        {/each}
        
        <!-- Display currently typing line -->
        {#if currentText || currentDots}
            <div class="leading-6">
                <span>{currentText}</span><span class="text-amber-200">{currentDots}</span>
                <span class="animate-pulse"></span>
            </div>
        {/if}
        
        <!-- Fill remaining space to maintain fixed height -->
        {#each Array(Math.max(0, 3 - displayedLines.length - (currentText || currentDots ? 1 : 0))) as _}
            <div class="leading-6">&nbsp;</div>
        {/each}
    </code>
</div>