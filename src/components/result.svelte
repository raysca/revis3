<script lang="ts">
	import Quiz from '$components/quiz.svelte';
	export let answers: Record<string, any>[];
	export let total: number;
	export let showQuizzes: boolean = true;

	const answersByTopic = answers.reduce((acc: Record<string, any>, answer: any) => {
		const topic = answer.quiz.topic?.title;
		if (!acc[topic]) {
			acc[topic] = [];
		}
		acc[topic].push(answer);
		return acc;
	}, {});

	const resultByTopic: Record<string, any> = Object.keys(answersByTopic).reduce((acc, topic) => {
		const total = answersByTopic[topic].length;
		const correct = answersByTopic[topic].filter((a: any) => a.correct).length;
		const percentage = Math.round((correct / total) * 100);
		const status = percentage > 50 ? 'passed' : 'failed';
		const progress = percentage > 50 ? 'text-info' : 'text-error';
		return Object.assign(acc, {
			[topic]: {
				total,
				correct,
				percentage,
				status,
				progress
			}
		});
	}, {});
</script>

<div class="card w-auto bg-base-100 shadow-xl rounded-none">
	<div class="card-body">
		<div class="card-title">
			<span>{total} Total Questions</span>
		</div>
		<div class="flex flex-col space-y-4">
			<table class="table table-zebra">
				<thead>
					<tr>
						<th>Topic</th>
						<th>Total</th>
						<th>Correct</th>
					</tr>
				</thead>
				<tbody>
					{#each Object.keys(answersByTopic) as topic}
						<tr class="font-semibold">
							<td class={resultByTopic[topic].progress}>{@html topic}</td>
							<td>{resultByTopic[topic].total}</td>
							<td>{resultByTopic[topic].correct}</td>
						</tr>
					{/each}
				</tbody>
			</table>
		</div>
		{#if showQuizzes}
			<div class="card-actions justify-end">
				<button class="btn btn-accent rounded-none" on:click={() => window.location.reload()}
					>Restart Test</button
				>
			</div>
		{/if}
	</div>
</div>

{#if showQuizzes}
	<div class="flex flex-col space-y-4">
		{#each answers as answer}
			<Quiz quiz={answer.quiz} choices={answer.choices} showComment={true} />
		{/each}
	</div>
{/if}
