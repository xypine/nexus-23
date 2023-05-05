<script>
	import Questionaire from "$lib/components/questionaire.svelte";
	import improvement_ideas from "$lib/improvement_ideas.json";
</script>

<Questionaire
	questions={improvement_ideas}
	questionnaire_name="Personal AI"
	show_progress={false}
	result_link="/view/all_done"
/>
