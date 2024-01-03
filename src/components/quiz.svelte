<script lang="ts">
	import { shuffle } from '$lib/shuffle';
	import type { Quiz } from '$lib/topic';

	export let quiz: Quiz;
	export const showComment: boolean = false;
	export const choices: string[] = [];

	let inputType = quiz?.answers.length > 1 ? 'checkbox' : 'radio';
</script>

<div class="w-auto mx-4">
	<p class="text-sm block badge badge-neutral">{@html quiz.topic?.title}</p>
	<h2 class="text-2xl font-semibold">{@html quiz.title}</h2>
	<p class="w-auto">
		{#each quiz.body as c}
			{@html c}
		{/each}
	</p>
	<p />
	<ul class="my-4 flex flex-col space-y-4">
		{#each shuffle(quiz.options) as option, index (option)}
			<li class="max-w-full">
				<label
					for={`${quiz.id}-${index}`}
					class="label cursor-pointer justify-start space-x-4 border px-4 w-full"
				>
					<input
						id={`${quiz.id}-${index}`}
						type={inputType}
						name="answer"
						class={`${inputType} ${inputType}-accent`}
						value={option}
					/>
					<span>{@html option}</span>
				</label>
			</li>
		{/each}
	</ul>
	{#if showComment && quiz.comment.length > 0}
		<div class="w-auto my-4">
			<h3 class="text-2xl">Explanation</h3>
			{#each quiz.comment as c}
				{@html c}
			{/each}
		</div>
	{/if}
</div>