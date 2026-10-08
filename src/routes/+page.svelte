<script lang="ts">
	import {
		Accordion,
		AccordionItem,
		Alert,
		Badge,
		Button,
		Card,
		Checkbox,
		Container,
		DarkMode,
		Divider,
		Form,
		Input,
		Select,
		notify
	} from 'theui-svelte';

	let name = $state('');
	let plan = $state('');
	let agreed = $state(false);

	const plans = [
		{ text: 'Starter', value: 'starter' },
		{ text: 'Team', value: 'team' },
		{ text: 'Enterprise', value: 'enterprise' }
	];

	// notify() is browser only - during server rendering it returns an empty string
	// and does nothing, so it is called from a handler rather than at the top level.
	function submit(event: SubmitEvent) {
		event.preventDefault();
		if (!agreed) {
			notify('Accept the terms and try again', 'warning');
			return;
		}
		notify(name ? `Thanks, ${name}` : 'Form submitted', 'success');
	}
</script>

<svelte:head>
	<title>theui-svelte starter</title>
	<meta
		name="description"
		content="A SvelteKit starter wired up with theui-svelte and Tailwind CSS v4."
	/>
</svelte:head>

<Container>
	<header class="flex flex-wrap items-start justify-between gap-6">
		<div>
			<Badge rounded="full">theui-svelte 3.1.0</Badge>
			<h1 class="mt-4 text-4xl font-semibold tracking-tight">Your starter is wired up</h1>
			<p class="mt-3 max-w-xl text-muted">
				Everything below is a theui-svelte component reading its colors from your stylesheet. If the
				buttons are styled and the toggle switches the theme, the setup is correct. Delete this page
				and start building.
			</p>
		</div>
		<DarkMode data-tooltip="Switch theme" />
	</header>

	<div class="my-12"><Divider /></div>

	<div class="grid gap-6 lg:grid-cols-2">
		<Card title="Buttons">
			<p class="mb-5 text-muted">
				Five colors across three themes, plus outline. Any <code>class</code> you pass is merged with
				tailwind-merge, so your own styling wins.
			</p>
			<div class="flex flex-wrap items-center gap-3">
				<Button color="brand">Brand</Button>
				<Button color="brand" theme="soft">Soft</Button>
				<Button color="brand" theme="gradient">Gradient</Button>
				<Button color="brand" outline>Outline</Button>
				<Button color="brand" loading loadingText="Saving" />
			</div>
		</Card>

		<Card title="Notifications and tooltips">
			<p class="mb-5 text-muted">
				<code>notify()</code> posts into the single <code>Notification</code> rendered in the layout.
				Hover the theme toggle above for a tooltip.
			</p>
			<div class="flex flex-wrap gap-3">
				<Button color="success" theme="soft" onclick={() => notify('Saved', 'success')}>
					Success
				</Button>
				<Button color="info" theme="soft" onclick={() => notify('Heads up', 'info')}>Info</Button>
				<Button color="warning" theme="soft" onclick={() => notify('Careful', 'warning')}>
					Warning
				</Button>
				<Button color="error" theme="soft" onclick={() => notify('Something broke', 'error')}>
					Error
				</Button>
			</div>
		</Card>
	</div>

	<div class="mt-6 grid gap-6 lg:grid-cols-2">
		<Card title="A form">
			<p class="mb-5 text-muted">
				<code>Form</code> passes its <code>variant</code>, <code>size</code> and
				<code>rounded</code> down to every control, so you set them once. If these inputs look
				styled, the bundled forms plugin is loading.
			</p>
			<Form variant="bordered" size="md" onsubmit={submit}>
				<Input type="text" name="name" bind:value={name} helperText="Used in the notification.">
					Your name
				</Input>

				<Select label="Plan" name="plan" placeholder="Choose a plan" options={plans} bind:value={plan} />

				<Checkbox name="terms" bind:checked={agreed}>I accept the terms</Checkbox>

				<Button type="submit" color="brand">Submit</Button>
			</Form>
		</Card>

		<div class="flex flex-col gap-6">
			<Alert type="info" theme="soft" variant="borderStart" icon dismissible>
				Alerts come in four types across two themes and four variants, and this one is dismissible.
			</Alert>

			<Card title="Where to go next">
				<Accordion rounded="md">
					<AccordionItem title="Add a component" open>
						Components are named exports: <code>import &lbrace; Modal &rbrace; from 'theui-svelte'</code>.
						All 70 are listed in the documentation with live examples.
					</AccordionItem>
					<AccordionItem title="Change the brand color">
						Override <code>--color-brand-500</code> in an <code>@theme</code> block in
						<code>src/routes/layout.css</code>. Every component follows it.
					</AccordionItem>
					<AccordionItem title="Point your AI assistant at the rules">
						Run <code>npx theui ai</code> once. It writes a pointer to the rules that ship inside
						the package into whichever instruction files your project already uses.
					</AccordionItem>
				</Accordion>
			</Card>
		</div>
	</div>

	<div class="my-12"><Divider /></div>

	<footer class="flex flex-wrap items-center gap-x-6 gap-y-2 text-muted">
		<a class="hover:text-default" href="https://www.theui.dev">Documentation</a>
		<a class="hover:text-default" href="https://www.theui.dev/docs/installation">Installation</a>
		<a class="hover:text-default" href="https://github.com/mbparvezme/theui-svelte">GitHub</a>
		<a class="hover:text-default" href="https://www.npmjs.com/package/theui-svelte">npm</a>
	</footer>
</Container>
