# SAKE Migration Progress

Migrating `wip-gcp-project-key-rotation` (original) to match `wip-gcp-project-key-rotation_test2`.

**What changed vs. the old plan:** target is now `_test2`, which implements the new 2-step flow (requester provides OAuth2 Client ID upfront, SAK assembled immediately, no hand-back from supporter).

Both repos point to the same remote: `git@github.com:bonprix/wip-gcp-project-key-rotation.git`
All changes are applied **manually** to the original.

---

## Overview

| Step | What | Status |
|---|---|---|
| 1 | Config files (package.json, vite, workspace, app.html) | ⏳ TODO |
| 2 | Delete old files | ⏳ TODO |
| 3 | Create new files (app.css, components, logic files) | ⏳ TODO |
| 4 | Rewrite existing pages | ⏳ TODO |
| 5 | Install & verify | ⏳ TODO |

**Files kept unchanged:** `src/lib/common-helpers.ts`, `src/lib/externally-sourced.ts`, `src/lib/index.ts`, `src/lib/request-helpers.ts`, `src/lib/upload-helpers.ts`, `src/lib/url-handling.ts`, `src/routes/+layout.js`, `src/app.d.ts`

---

## Step 1 — Config files ⏳ TODO

### `webapp/package.json`

Two dependency changes:
- Remove `"@sveltestrap/sveltestrap": "^7.1.0"` from `dependencies`
- Add `"@lucide/svelte": "^0.511.0"` to `dependencies`
- Add `"@tailwindcss/vite": "^4.1.4"` to `devDependencies`

```json
"devDependencies": {
    ...
    "tailwindcss": "^4.1.4",
    "@tailwindcss/vite": "^4.1.4",
    ...
},
"dependencies": {
    "@lucide/svelte": "^0.511.0",
    "@tailwindcss/container-queries": "^0.1.1",
    ...
}
```

---

### `webapp/vite.config.ts`

Add the Tailwind Vite plugin (required for Tailwind v4 — without it, no CSS is generated):

```ts
import { sveltekit } from '@sveltejs/kit/vite';
import tailwindcss from '@tailwindcss/vite';
import { defineConfig } from 'vitest/config';

export default defineConfig({
	plugins: [tailwindcss(), sveltekit()],
	test: {
		include: ['src/**/*.{test,spec}.{js,ts}']
	},
	server: {
		allowedHosts: ['localhost', '127.0.0.1', 'sake-dev.psi.gcns.bonprix.net']
	}
});
```

---

### `webapp/pnpm-workspace.yaml`

Add `allowBuilds` section (required in pnpm 11 — the placeholder `set this to true or false` blocks esbuild's native binary build):

```yaml
allowBuilds:
  esbuild: true

minimumReleaseAge: 10080

onlyBuiltDependencies:
  - '@sveltejs/kit'
  - esbuild
```

---

### `webapp/src/app.html`

Replace the Bootstrap body classes with a clean layout. Add Inter font preconnect links:

```html
<!doctype html>
<html lang="en">
	<head>
		<meta charset="utf-8" />
		<link rel="icon" href="%sveltekit.assets%/favicon.png" />
		<meta name="viewport" content="width=device-width, initial-scale=1" />
		<link rel="preconnect" href="https://fonts.googleapis.com" />
		<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
		<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet" />
		<script src="https://apis.google.com/js/api.js"></script>

		%sveltekit.head%
	</head>
	<body data-sveltekit-preload-data="hover">
		<div style="display: contents">%sveltekit.body%</div>
		<script src="https://accounts.google.com/gsi/client"></script>
	</body>
</html>
```

---

### `webapp/src/routes/+layout.js` — verify, do not modify

This file is kept unchanged. Before starting the migration, confirm it contains exactly:

```js
export const trailingSlash = 'always';
export const prerender = true;
export const ssr = false;
```

These three settings are required for the static adapter deployment. If the file differs, investigate before proceeding.

---

## Step 2 — Delete old files ⏳ TODO

These files are not used in the new design:

```bash
rm webapp/src/lib/KeyManagementForm.svelte
rm webapp/src/lib/RequestForm.svelte
rm webapp/src/lib/RequestOptions.svelte
rm webapp/src/lib/TestForm.svelte
rm webapp/src/routes/request/dynamic/+page.svelte
rm webapp/src/routes/request/static/+page.svelte
rmdir webapp/src/routes/request/dynamic
rmdir webapp/src/routes/request/static
```

> ⚠️ **Expected:** after this step, `pnpm check` will report TypeScript errors because the existing page files still import the deleted components. This is normal — the errors will be resolved once Step 4 (page rewrites) is complete. Do not try to fix them here.

---

## Step 3 — Create new files ⏳ TODO

### File 1/5: `webapp/src/app.css`

Global CSS entry point. Tailwind v4 with full component library:

```css
@import 'tailwindcss';

@layer base {
	body {
		font-family: 'Inter', sans-serif;
	}
}

@layer components {
	.page { @apply min-h-screen bg-slate-100; }
	.content-wrap { @apply mx-auto max-w-2xl px-6 py-5; }

	.card { @apply rounded-2xl border border-slate-200 bg-white shadow-sm; }
	.card-pad { @apply rounded-2xl border border-slate-200 bg-white shadow-sm p-6; }
	.card-pad-sm { @apply rounded-2xl border border-slate-200 bg-white shadow-sm p-5; }

	.section-label { @apply text-xs font-semibold uppercase tracking-widest text-slate-400; }
	.field-label { @apply mb-1.5 block text-sm font-medium text-slate-700; }

	.input { @apply w-full rounded-lg border border-slate-300 px-3.5 py-2.5 text-sm text-slate-900 transition focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-500; }
	.input-mono { @apply w-full rounded-lg border border-slate-300 px-3.5 py-2.5 font-mono text-sm text-slate-900 transition focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-500; }
	.textarea-normal { @apply w-full resize-none rounded-xl border border-slate-300 px-3.5 py-2.5 font-mono text-sm text-slate-900 transition focus:border-blue-400 focus:outline-none focus:ring-2 focus:ring-blue-500; }
	.textarea-error { @apply w-full resize-none rounded-xl border border-red-400 px-3.5 py-2.5 font-mono text-sm text-slate-900 ring-2 ring-red-100 transition focus:outline-none; }

	.btn-primary { @apply flex w-full items-center justify-center gap-2 rounded-lg bg-blue-600 px-4 py-2.5 text-sm font-semibold text-white transition-colors hover:bg-blue-700 disabled:cursor-not-allowed disabled:opacity-50; }
	.btn-dark { @apply flex w-full items-center justify-center gap-2 rounded-lg bg-slate-800 px-4 py-2.5 text-sm font-semibold text-white transition-colors hover:bg-slate-900; }
	.btn-copy { @apply flex flex-shrink-0 items-center gap-1.5 rounded-lg border border-slate-300 bg-white px-3 py-2 text-xs font-semibold text-slate-600 transition-colors hover:bg-slate-50; }
	.btn-copy-xl { @apply flex flex-shrink-0 items-center gap-1.5 rounded-xl border border-slate-300 bg-white px-3 py-2 text-xs font-semibold text-slate-600 transition-colors hover:bg-slate-50; }

	.badge-green { @apply inline-flex items-center rounded-full bg-green-100 px-1.5 py-0.5 text-xs font-medium text-green-700; }
	.badge-red { @apply inline-flex items-center gap-1 rounded-full bg-red-100 px-2 py-0.5 text-xs font-medium text-red-600; }
	.badge-blue { @apply inline-flex items-center rounded bg-blue-100 px-2 py-0.5 text-xs font-semibold text-blue-700; }

	.warning-banner { @apply flex items-center gap-2.5 rounded-xl border border-amber-200 bg-amber-50 p-3.5; }
	.warning-box { @apply mt-4 flex items-start gap-2.5 rounded-xl border border-amber-200 bg-amber-50 p-3.5; }

	.value-box { @apply flex-1 rounded-xl border border-slate-200 bg-slate-50 px-3.5 py-2.5 font-mono text-sm text-slate-800; }

	.detail-row { @apply flex items-center justify-between border-b border-slate-50 py-2.5; }
	.detail-row-top { @apply flex items-start justify-between border-b border-slate-50 py-2.5; }
	.avatar { @apply flex h-9 w-9 flex-shrink-0 items-center justify-center rounded-full bg-blue-600 text-sm font-bold text-white; }
	.hint { @apply mt-2 text-center text-xs text-slate-400; }

	.th { @apply px-3 py-2.5 text-left text-xs font-semibold text-slate-500; }
	.td { @apply px-3 py-2.5 text-xs text-slate-500; }
	.td-mono { @apply px-3 py-2.5 font-mono text-xs text-slate-700; }

	.role-card { @apply group block rounded-2xl border-2 border-slate-200 bg-white p-5 transition-all hover:border-blue-400 hover:shadow-md; }
	.step-num { @apply mt-0.5 flex h-5 w-5 flex-shrink-0 items-center justify-center rounded-full bg-blue-100 text-xs font-bold text-blue-700; }

	.btn-option { @apply group flex w-full items-center gap-3 rounded-xl border-2 border-slate-200 bg-white p-4 text-left transition-all; }
}
```

---

### File 2/5: `webapp/src/lib/components/StepIndicator.svelte`

Create directory `webapp/src/lib/components/` first (it doesn't exist yet).

```svelte
<script lang="ts">
	import { Check } from '@lucide/svelte';

	interface Step { label: string; }
	interface Props { steps: Step[]; current: number; }

	let { steps, current }: Props = $props();

	function circleClass(done: boolean, active: boolean): string {
		const base = 'flex h-6 w-6 flex-shrink-0 items-center justify-center rounded-full text-xs font-bold';
		if (done) return `${base} bg-green-500 text-white`;
		if (active) return `${base} bg-blue-600 text-white ring-4 ring-blue-100`;
		return `${base} bg-slate-200 text-slate-500`;
	}

	function labelClass(done: boolean, active: boolean): string {
		const base = 'hidden text-xs sm:block';
		if (done) return `${base} font-medium text-green-600`;
		if (active) return `${base} font-semibold text-slate-900`;
		return `${base} text-slate-400`;
	}
</script>

<div class="flex items-center gap-1.5">
	{#each steps as step, i}
		{@const n = i + 1}
		{@const done = current > n}
		{@const active = current === n}
		<div class="flex items-center gap-1.5">
			<div class={circleClass(done, active)}>
				{#if done}
					<Check class="h-3.5 w-3.5" />
				{:else}
					{n}
				{/if}
			</div>
			<span class={labelClass(done, active)}>{step.label}</span>
		</div>
		{#if i < steps.length - 1}
			<div class="mx-1.5 h-px min-w-[12px] flex-1 bg-slate-200"></div>
		{/if}
	{/each}
</div>
```

---

### File 3/5: `webapp/src/lib/components/TopBar.svelte`

```svelte
<script lang="ts">
	import { ArrowLeft } from '@lucide/svelte';
	import StepIndicator from './StepIndicator.svelte';

	interface Props {
		title: string;
		backHref?: string;
		steps?: { label: string }[];
		currentStep?: number;
	}

	let { title, backHref = '/', steps, currentStep = 1 }: Props = $props();
</script>

<div class="sticky top-0 z-10 border-b border-slate-200 bg-white shadow-sm">
	<div class="mx-auto flex max-w-2xl items-center gap-3 px-6 py-3.5">
		<a href={backHref} class="flex items-center gap-1 text-sm text-slate-400 transition-colors hover:text-slate-700">
			<ArrowLeft class="h-4 w-4" />
			Back
		</a>
		<span class="select-none text-slate-200">|</span>
		<h1 class="text-sm font-semibold text-slate-700">{title}</h1>
	</div>
	{#if steps}
		<div class="mx-auto max-w-2xl px-6 pb-3.5">
			<StepIndicator {steps} current={currentStep} />
		</div>
	{/if}
</div>
```

---

### File 4/5: `webapp/src/lib/components/KeysTable.svelte`

```svelte
<script lang="ts">
	import { Trash2 } from '@lucide/svelte';

	interface Key { id: string; created: string; expires: string; expired: boolean; }

	let { keys, onDelete }: { keys: Key[]; onDelete: (id: string) => void } = $props();
</script>

<div class="overflow-hidden rounded-xl border border-slate-200">
	<table class="w-full text-sm">
		<thead>
			<tr class="border-b border-slate-200 bg-slate-50">
				<th class="th">Key ID</th>
				<th class="th">Created</th>
				<th class="th">Expires</th>
				<th class="th">Status</th>
				<th class="px-3 py-2.5"></th>
			</tr>
		</thead>
		<tbody>
			{#each keys as key (key.id)}
				<tr class="border-b border-slate-100 last:border-0">
					<td class="td-mono">{key.id}</td>
					<td class="td">{key.created}</td>
					<td class="td">{key.expires}</td>
					<td class="td">
						<span class={key.expired ? 'badge-red' : 'badge-green'}>
							{key.expired ? 'Expired' : 'Active'}
						</span>
					</td>
					<td class="td text-right">
						<button onclick={() => onDelete(key.id)}
							class="inline-flex items-center gap-1 text-xs font-medium text-red-500 transition-colors hover:text-red-700">
							<Trash2 class="h-3.5 w-3.5" />
							Delete
						</button>
					</td>
				</tr>
			{:else}
				<tr>
					<td colspan="5" class="px-3 py-4 text-center text-xs text-slate-400">No existing keys</td>
				</tr>
			{/each}
		</tbody>
	</table>
</div>
```

---

### File 5/5: `webapp/src/routes/request/logic.svelte.ts`

New file — contains all crypto/state logic for the 2-step requester flow:

```ts
import * as pkijs from 'pkijs';
import * as asn1js from 'asn1js';
import { arrayBufferToString, toBase64 } from 'pvutils';
import { formatPEM, sha1 } from '$lib/externally-sourced';
import { decomposeServiceAccount } from '$lib/common-helpers';
import { base } from '$app/paths';

export function createRequestState() {
	let currentStep = $state(1);
	let jiraTicket = $state('');
	let saEmail = $state('');
	let oauth2ClientId = $state('');
	let expiryDays = $state(90);
	let generating = $state(false);
	let linkCopied = $state(false);

	let certPem = $state('');
	let computedKeyId = $state('');
	let sakJson = $state('');
	let sakFilename = $state('');

	let parsedProject = $derived(saEmail.match(/@(.+?)\.iam\.gserviceaccount\.com/)?.[1] ?? null);
	let parsedName = $derived(saEmail.match(/^([^@]+)@/)?.[1] ?? null);
	let emailValid = $derived(parsedProject !== null && parsedName !== null);

	let validFrom = $derived(
		new Date().toLocaleDateString('de-DE', { day: '2-digit', month: '2-digit', year: 'numeric' })
	);
	let validTo = $derived(
		(() => {
			const d = new Date();
			d.setDate(d.getDate() + expiryDays);
			return d.toLocaleDateString('de-DE', { day: '2-digit', month: '2-digit', year: 'numeric' });
		})()
	);

	let shareLink = $derived(
		certPem
			? `${window.location.origin}${base}/upload/?ticket=${encodeURIComponent(jiraTicket)}&sa=${encodeURIComponent(saEmail)}&days=${expiryDays}#cert=${encodeURIComponent(certPem)}`
			: ''
	);

	function goToStep(n: number) {
		currentStep = n;
		window.scrollTo({ top: 0, behavior: 'smooth' });
	}

	async function generateCert() {
		if (!jiraTicket.trim() || !emailValid || !oauth2ClientId.trim()) return;
		generating = true;

		try {
			const crypto = pkijs.getCrypto(true);
			const certificate = new pkijs.Certificate();
			certificate.version = 2;
			certificate.serialNumber = new asn1js.Integer({
				valueHex: pkijs.getRandomValues(new Uint8Array(10)).buffer
			});

			const notBefore = new Date();
			const notAfter = new Date();
			notAfter.setUTCDate(notAfter.getDate() + expiryDays);
			certificate.notBefore.value = notBefore;
			certificate.notAfter.value = notAfter;

			function setDN(target: pkijs.RelativeDistinguishedNames, cn: string, o: string) {
				target.typesAndValues.push(
					new pkijs.AttributeTypeAndValue({
						type: '2.5.4.3',
						value: new asn1js.PrintableString({ value: cn })
					})
				);
				target.typesAndValues.push(
					new pkijs.AttributeTypeAndValue({
						type: '2.5.4.10',
						value: new asn1js.PrintableString({ value: o })
					})
				);
			}

			setDN(certificate.subject, saEmail, jiraTicket);
			setDN(certificate.issuer, saEmail, jiraTicket);

			certificate.extensions = [];
			const basicConstr = new pkijs.BasicConstraints({ cA: false, pathLenConstraint: 0 });
			certificate.extensions.push(
				new pkijs.Extension({
					extnID: '2.5.29.19',
					critical: false,
					extnValue: basicConstr.toSchema().toBER(false),
					parsedValue: basicConstr
				})
			);

			const algorithm = pkijs.getAlgorithmParameters('RSASSA-PKCS1-v1_5', 'generateKey');
			if ('hash' in algorithm.algorithm) {
				(algorithm.algorithm.hash as { name: string }).name = 'SHA-256';
			}
			const keys = await crypto.generateKey(
				algorithm.algorithm as AlgorithmIdentifier,
				true,
				algorithm.usages
			);

			await certificate.subjectPublicKeyInfo.importKey(keys.publicKey);
			await certificate.sign(keys.privateKey, 'SHA-256');

			const fppk = await crypto.exportKey('pkcs8', keys.privateKey);
			const raw = certificate.toSchema().toBER();

			certPem = `-----BEGIN CERTIFICATE-----\n${formatPEM(toBase64(arrayBufferToString(raw)))}\n-----END CERTIFICATE-----`;
			const privateKeyPem = `-----BEGIN PRIVATE KEY-----\n${formatPEM(toBase64(arrayBufferToString(fppk)))}\n-----END PRIVATE KEY-----`;

			computedKeyId = await sha1(certPem + '\n');

			const { project } = decomposeServiceAccount(saEmail);
			const sak = {
				type: 'service_account',
				project_id: project,
				private_key_id: computedKeyId,
				private_key: privateKeyPem + '\n',
				client_email: saEmail,
				client_id: oauth2ClientId,
				auth_uri: 'https://accounts.google.com/o/oauth2/auth',
				token_uri: 'https://oauth2.googleapis.com/token',
				auth_provider_x509_cert_url: 'https://www.googleapis.com/oauth2/v1/certs',
				client_x509_cert_url: `https://www.googleapis.com/robot/v1/metadata/x509/${encodeURIComponent(saEmail)}`,
				universe_domain: 'googleapis.com'
			};
			sakJson = JSON.stringify(sak, null, 2);

			const nowIso = notBefore.toISOString().replaceAll(':', '_').replaceAll('.', '_');
			const expIso = notAfter.toISOString().replaceAll(':', '_').replaceAll('.', '_');
			sakFilename = `${saEmail.replace('@', '_').replaceAll('.', '_')}_from_${nowIso}_to_${expIso}`;

			goToStep(2);
		} finally {
			generating = false;
		}
	}

	async function copyLink() {
		await navigator.clipboard.writeText(shareLink);
		linkCopied = true;
		setTimeout(() => (linkCopied = false), 1800);
	}

	function downloadSAK() {
		const blob = new Blob([sakJson], { type: 'application/json' });
		const url = URL.createObjectURL(blob);
		const a = document.createElement('a');
		a.href = url;
		a.download = `${sakFilename}.json`;
		a.click();
		URL.revokeObjectURL(url);
	}

	return {
		get currentStep() { return currentStep; },
		get jiraTicket() { return jiraTicket; },
		set jiraTicket(v: string) { jiraTicket = v; },
		get saEmail() { return saEmail; },
		set saEmail(v: string) { saEmail = v; },
		get oauth2ClientId() { return oauth2ClientId; },
		set oauth2ClientId(v: string) { oauth2ClientId = v; },
		get expiryDays() { return expiryDays; },
		set expiryDays(v: number) { expiryDays = v; },
		get generating() { return generating; },
		get linkCopied() { return linkCopied; },
		get computedKeyId() { return computedKeyId; },
		get sakFilename() { return sakFilename; },
		get parsedProject() { return parsedProject; },
		get parsedName() { return parsedName; },
		get emailValid() { return emailValid; },
		get validFrom() { return validFrom; },
		get validTo() { return validTo; },
		get shareLink() { return shareLink; },
		goToStep,
		generateCert,
		copyLink,
		downloadSAK
	};
}
```

---

### File 6/6 (bonus): `webapp/src/routes/upload/logic.svelte.ts`

New file — upload state factory. Note the hardcoded `OAUTH_CLIENT_ID` constant (the app's own GCP client ID for the Google Sign-In button — not the requester's SA client ID):

```ts
import { parseCert } from '$lib/upload-helpers';

export type UploadStep = '1' | '2a' | '2b' | '3';

export interface ExistingKey {
	id: string;
	name: string;
	created: string;
	expires: string;
	expired: boolean;
}

const OAUTH_CLIENT_ID = '162427647549-i9p41figpgdicigpfjnmemuj6j4etkln.apps.googleusercontent.com';

export function createUploadState() {
	let currentStep = $state<UploadStep>('1');
	let saEmail = $state('');
	let jiraTicket = $state('');
	let expiryDays = $state(90);
	let certPem = $state('');
	let certValidFrom = $state<Date | null>(null);
	let certValidTo = $state<Date | null>(null);
	let existingKeys = $state<ExistingKey[]>([]);
	let uploading = $state(false);
	let signedInEmail = $state('');
	let cliCopied = $state(false);

	let tokenClient: google.accounts.oauth2.TokenClient | null = null;
	let gapiReady = false;

	let parsedProject = $derived(saEmail.match(/@(.+?)\.iam\.gserviceaccount\.com/)?.[1] ?? '—');
	let displayStep = $derived<number>(
		currentStep === '1' ? 1 : currentStep === '2a' || currentStep === '2b' ? 2 : 3
	);

	let validFrom = $derived(
		certValidFrom
			? certValidFrom.toLocaleDateString('de-DE', { day: '2-digit', month: '2-digit', year: 'numeric' })
			: new Date().toLocaleDateString('de-DE', { day: '2-digit', month: '2-digit', year: 'numeric' })
	);
	let validTo = $derived(
		certValidTo
			? certValidTo.toLocaleDateString('de-DE', { day: '2-digit', month: '2-digit', year: 'numeric' })
			: (() => {
					const d = new Date();
					d.setDate(d.getDate() + expiryDays);
					return d.toLocaleDateString('de-DE', { day: '2-digit', month: '2-digit', year: 'numeric' });
				})()
	);

	let cliCommand = $derived(
		`TMPF=$(mktemp) && \\
echo -e "-----BEGIN CERTIFICATE-----\\n${certPem}" > $TMPF && \\
gcloud --project ${parsedProject} \\
  iam service-accounts keys upload $TMPF \\
  --iam-account=${saEmail} && \\
rm $TMPF`
	);

	function goToStep(step: UploadStep) {
		currentStep = step;
		window.scrollTo({ top: 0, behavior: 'smooth' });
	}

	function parseFromUrl() {
		const params = new URLSearchParams(window.location.search);
		const hashParams = new URLSearchParams(window.location.hash.substring(1));
		const certText = hashParams.get('cert');

		if (certText) {
			try {
				const parsed = parseCert(certText);
				certPem = parsed.certText;
				saEmail = parsed.samail;
				jiraTicket = parsed.jiraTicket ?? params.get('ticket') ?? '';
				certValidFrom = parsed.certBegin;
				certValidTo = parsed.certEnd;
				expiryDays = parsed.certDuration ?? Number(params.get('days') ?? 90);
			} catch {
				saEmail = params.get('sa') ?? '';
				jiraTicket = params.get('ticket') ?? '';
				expiryDays = Number(params.get('days') ?? 90);
			}
		} else {
			saEmail = params.get('sa') ?? '';
			jiraTicket = params.get('ticket') ?? '';
			expiryDays = Number(params.get('days') ?? 90);
		}

		tokenClient = google.accounts.oauth2.initTokenClient({
			client_id: OAUTH_CLIENT_ID,
			scope: 'https://www.googleapis.com/auth/cloud-platform',
			callback: async (tokenResponse) => {
				gapi.load('client', async () => {
					gapi.client.setToken(tokenResponse);
					await gapi.client.init({
						discoveryDocs: ['https://iam.googleapis.com/$discovery/rest?version=v1']
					});
					gapiReady = true;

					try {
						const info = await fetch('https://www.googleapis.com/oauth2/v3/userinfo', {
							headers: { Authorization: `Bearer ${tokenResponse.access_token}` }
						});
						const user = await info.json();
						signedInEmail = user.email ?? 'authenticated';
					} catch {
						signedInEmail = 'authenticated';
					}

					await loadExistingKeys();
					goToStep('2a');
				});
			}
		});
	}

	async function loadExistingKeys() {
		if (!saEmail || !gapiReady) return;
		try {
			const result = await gapi.client.iam.projects.serviceAccounts.keys.list({
				name: `projects/-/serviceAccounts/${saEmail}`
			});
			existingKeys = ((result.result.keys as gapi.client.iam.ServiceAccountKey[]) ?? [])
				.filter((k) => k.keyType === 'USER_MANAGED')
				.map((k) => {
					const expires = new Date(k.validBeforeTime!);
					return {
						id: k.name!.split('/').pop() as string,
						name: k.name!,
						created: new Date(k.validAfterTime!).toLocaleDateString('de-DE'),
						expires: expires.toLocaleDateString('de-DE'),
						expired: expires < new Date()
					};
				});
		} catch (e) {
			console.error('Failed to load keys:', e);
		}
	}

	async function deleteKey(id: string) {
		const key = existingKeys.find((k) => k.id === id);
		if (!key) return;
		if (gapiReady) {
			try {
				await gapi.client.iam.projects.serviceAccounts.keys.delete({ name: key.name });
			} catch (e) {
				console.error('Failed to delete key:', e);
				return;
			}
		}
		existingKeys = existingKeys.filter((k) => k.id !== id);
	}

	async function signInWithGoogle() {
		tokenClient?.requestAccessToken();
	}

	function switchAccount() {
		signedInEmail = '';
		gapiReady = false;
		goToStep('1');
	}

	async function browserUpload() {
		if (!gapiReady || !certPem) return;
		uploading = true;
		try {
			await gapi.client.iam.projects.serviceAccounts.keys.upload({
				name: `projects/-/serviceAccounts/${saEmail}`,
				resource: { publicKeyData: btoa(certPem) }
			});
			goToStep('3');
		} catch (e) {
			console.error('Upload failed:', e);
		} finally {
			uploading = false;
		}
	}

	async function copyCLI() {
		await navigator.clipboard.writeText(cliCommand);
		cliCopied = true;
		setTimeout(() => (cliCopied = false), 1800);
	}

	return {
		get currentStep() { return currentStep; },
		get displayStep() { return displayStep; },
		get saEmail() { return saEmail; },
		get jiraTicket() { return jiraTicket; },
		get expiryDays() { return expiryDays; },
		get parsedProject() { return parsedProject; },
		get validFrom() { return validFrom; },
		get validTo() { return validTo; },
		get existingKeys() { return existingKeys; },
		get uploading() { return uploading; },
		get cliCommand() { return cliCommand; },
		get cliCopied() { return cliCopied; },
		get signedInEmail() { return signedInEmail; },
		goToStep,
		parseFromUrl,
		deleteKey,
		signInWithGoogle,
		switchAccount,
		browserUpload,
		copyCLI
	};
}
```

---

## Step 4 — Rewrite existing pages ⏳ TODO

### `webapp/src/routes/+layout.svelte`

Replace the entire file (remove sveltestrap, setContext, Instance UUID display):

```svelte
<script lang="ts">
	import '../app.css';
	let { children } = $props();
</script>

{@render children()}
```

---

### `webapp/src/routes/+page.svelte`

Replace the entire file:

```svelte
<script lang="ts">
	import { Key, FileText, CloudUpload, ArrowRight, TriangleAlert } from '@lucide/svelte';
</script>

<svelte:head>
	<title>SAKE — Service Account Key Exchange Helper</title>
</svelte:head>

<div class="flex min-h-screen items-center justify-center bg-slate-100 p-8">
	<div class="w-full max-w-lg">
		<div class="mb-10 text-center">
			<div class="mb-4 inline-flex h-14 w-14 items-center justify-center rounded-2xl bg-blue-600 shadow-md">
				<Key class="h-7 w-7 text-white" />
			</div>
			<h1 class="text-2xl font-bold tracking-tight text-slate-900">SAKE</h1>
			<p class="mt-1 text-sm text-slate-500">Service Account Key Exchange Helper</p>
		</div>

		<div class="grid grid-cols-2 gap-3">
			<a href="/request/" class="role-card">
				<div class="mb-4 flex h-9 w-9 items-center justify-center rounded-xl bg-slate-100 transition-colors group-hover:bg-blue-50">
					<FileText class="h-5 w-5 text-slate-500 transition-colors group-hover:text-blue-600" />
				</div>
				<p class="mb-1 text-sm font-semibold leading-snug text-slate-900">I need a Service Account Key</p>
				<p class="mb-5 text-xs text-slate-400">Start the request process</p>
				<span class="inline-flex items-center gap-1.5 text-xs font-semibold text-blue-600">
					Request
					<ArrowRight class="h-3.5 w-3.5 transition-transform group-hover:translate-x-0.5" />
				</span>
			</a>

			<a href="/upload/" class="role-card">
				<div class="mb-4 flex h-9 w-9 items-center justify-center rounded-xl bg-slate-100 transition-colors group-hover:bg-blue-50">
					<CloudUpload class="h-5 w-5 text-slate-500 transition-colors group-hover:text-blue-600" />
				</div>
				<p class="mb-1 text-sm font-semibold leading-snug text-slate-900">I received a link from a requester</p>
				<p class="mb-5 text-xs text-slate-400">GCP access required</p>
				<span class="inline-flex items-center gap-1.5 text-xs font-semibold text-blue-600">
					Open Upload
					<ArrowRight class="h-3.5 w-3.5 transition-transform group-hover:translate-x-0.5" />
				</span>
			</a>
		</div>

		<div class="mt-4 flex items-start gap-3 rounded-xl border border-amber-200 bg-amber-50 p-4">
			<TriangleAlert class="mt-0.5 h-4 w-4 flex-shrink-0 text-amber-500" />
			<p class="text-sm leading-relaxed text-amber-800">
				<strong>Important:</strong> Do not close your browser tab once you start. Your private key only lives in memory — closing the tab means starting over.
			</p>
		</div>

		<div class="mb-4 mt-4 rounded-2xl border border-slate-200 bg-white p-6 shadow-sm">
			<p class="mb-4 text-xs font-semibold uppercase tracking-widest text-slate-400">How this works</p>
			<div class="grid grid-cols-2 gap-x-6 gap-y-2.5">
				<p class="text-sm font-semibold text-slate-800" style="grid-column:1;grid-row:1">You (Requester)</p>
				<p class="border-l border-slate-100 pl-6 text-sm font-semibold text-slate-800" style="grid-column:2;grid-row:1">
					Supporter <span class="ml-1 text-xs font-normal text-slate-400">GCP access req.</span>
				</p>
				<div class="flex items-start gap-2.5 text-sm text-slate-600" style="grid-column:1;grid-row:2">
					<span class="step-num">1</span>
					Fill in details + OAuth2 Client ID
				</div>
				<div class="flex items-start gap-2.5 text-sm text-slate-600" style="grid-column:1;grid-row:3">
					<span class="step-num">2</span>
					Download SAK &amp; share link
				</div>
				<p class="pt-1 text-xs font-medium text-green-600" style="grid-column:1;grid-row:4">Your key file is ready immediately.</p>
				<div class="flex items-start gap-2.5 border-l border-slate-100 pl-6 text-sm text-slate-600" style="grid-column:2;grid-row:5">
					<span class="step-num">3</span>
					Open your link
				</div>
				<div class="flex items-start gap-2.5 border-l border-slate-100 pl-6 text-sm text-slate-600" style="grid-column:2;grid-row:6">
					<span class="step-num">4</span>
					Activate cert in GCP
				</div>
				<p class="border-l border-slate-100 pl-6 text-xs text-slate-400" style="grid-column:2;grid-row:7">No response needed.</p>
			</div>
		</div>

		<p class="mt-8 text-center text-xs text-slate-300">Internal Tool · GCP Management</p>
	</div>
</div>
```

---

### `webapp/src/routes/request/+page.svelte`

Replace the entire file (new 2-step flow — OAuth2 Client ID in step 1, SAK download in step 2):

```svelte
<script lang="ts">
	import { TriangleAlert, CircleCheck, Loader2, ArrowRight, Check, Copy, Download } from '@lucide/svelte';
	import TopBar from '$lib/components/TopBar.svelte';
	import { createRequestState } from './logic.svelte';

	const STEPS = [{ label: 'Details' }, { label: 'Done' }];
	const r = createRequestState();
</script>

<svelte:head>
	<title>SAKE — Request a Service Account Key</title>
</svelte:head>

<div class="page">
	<TopBar title="Request a Service Account Key" steps={STEPS} currentStep={r.currentStep} />

	{#if r.currentStep === 2}
		<div class="mx-auto max-w-2xl px-6 pt-5">
			<div class="warning-banner">
				<TriangleAlert class="h-4 w-4 flex-shrink-0 text-amber-500" />
				<p class="text-xs font-semibold text-amber-800">
					Do not close this tab before downloading. Your private key only exists here in memory.
				</p>
			</div>
		</div>
	{/if}

	<div class="content-wrap">

		{#if r.currentStep === 1}
			<div class="card-pad">
				<h2 class="mb-5 text-base font-semibold text-slate-900">Enter your request details</h2>
				<div class="space-y-4">
					<div>
						<label for="jira-ticket" class="field-label">Jira Ticket <span class="text-red-400">*</span></label>
						<input id="jira-ticket" type="text" bind:value={r.jiraTicket} placeholder="e.g. TICKET-1234" class="input" />
					</div>
					<div>
						<label for="sa-email" class="field-label">Service Account Mail <span class="text-red-400">*</span></label>
						<input id="sa-email" type="text" bind:value={r.saEmail} placeholder="name@project.iam.gserviceaccount.com" class="input-mono" />
						{#if r.emailValid}
							<div class="mt-1.5 flex items-center gap-1.5">
								<CircleCheck class="h-3.5 w-3.5 flex-shrink-0 text-green-500" />
								<span class="text-xs text-slate-500">
									Project: <span class="font-medium text-slate-700">{r.parsedProject}</span>
								</span>
								<span class="text-slate-300">·</span>
								<span class="text-xs text-slate-500">
									Name: <span class="font-medium text-slate-700">{r.parsedName}</span>
								</span>
								<span class="ml-1 text-xs italic text-slate-400">parsed automatically</span>
							</div>
						{/if}
					</div>
					<div>
						<label for="oauth2-client-id" class="field-label">OAuth2 Client ID <span class="text-red-400">*</span></label>
						<input id="oauth2-client-id" type="text" bind:value={r.oauth2ClientId} placeholder="e.g. 111111222222333333444444" class="input-mono" />
						<p class="mt-1.5 text-xs text-slate-400">Find this in the GCP console under the service account details, or ask your team lead.</p>
					</div>
					<div>
						<label for="expiry-days" class="field-label">Key Expiry <span class="text-red-400">*</span></label>
						<div class="flex items-center gap-2">
							<input id="expiry-days" type="number" bind:value={r.expiryDays} min="1" max="90" class="input w-24" />
							<span class="text-sm text-slate-500">days (max 90)</span>
						</div>
						<p class="mt-1.5 text-xs text-slate-500">
							Expires on: <span class="font-medium text-slate-700">{r.validTo}</span>
						</p>
					</div>
				</div>
				<div class="mt-6 border-t border-slate-100 pt-5">
					<button onclick={r.generateCert}
						disabled={r.generating || !r.jiraTicket.trim() || !r.emailValid || !r.oauth2ClientId.trim()}
						class="btn-primary">
						{#if r.generating}
							<Loader2 class="h-4 w-4 animate-spin" />
							Generating…
						{:else}
							Generate Certificate &amp; Assemble Key
							<ArrowRight class="h-4 w-4" />
						{/if}
					</button>
					<p class="mt-2 text-center text-xs text-slate-400">Your browser generates a key pair locally. The private key never leaves this tab.</p>
				</div>
			</div>
		{/if}

		{#if r.currentStep === 2}
			<div class="card-pad mb-3">
				<div class="mb-5 flex items-start gap-3 border-b border-slate-100 pb-5">
					<div class="flex h-8 w-8 flex-shrink-0 items-center justify-center rounded-full bg-green-100">
						<Check class="h-4 w-4 text-green-600" />
					</div>
					<div>
						<p class="text-sm font-semibold text-slate-900">Certificate generated &amp; key assembled</p>
						<p class="mt-0.5 text-xs text-slate-500">
							Valid: <span class="text-slate-700">{r.validFrom}</span> — <span class="text-slate-700">{r.validTo}</span>
							<span class="badge-green ml-1.5">{r.expiryDays} days</span>
						</p>
					</div>
				</div>
				<p class="mb-1 text-sm font-semibold text-slate-900">Download your Service Account Key</p>
				<p class="mb-3 text-xs text-slate-500">Your SAK is ready. Download it now before you close this tab.</p>
				<button onclick={r.downloadSAK} class="btn-primary py-3">
					<Download class="h-5 w-5" />
					Download Service Account Key (.json)
				</button>
				<p class="mt-1.5 text-center text-xs text-slate-400">{r.sakFilename}.json</p>
			</div>

			<div class="card-pad mb-3">
				<p class="mb-1 text-sm font-semibold text-slate-900">Send activation link to your supporter</p>
				<p class="mb-3 text-xs text-slate-500">The supporter only needs to activate the cert in GCP — no response from them is needed.</p>
				<div class="flex items-stretch gap-2">
					<div class="flex min-w-0 flex-1 items-center truncate rounded-lg border border-slate-200 bg-slate-50 px-3 py-2.5 font-mono text-xs text-slate-600">
						{r.shareLink}
					</div>
					<button onclick={r.copyLink} class="btn-copy">
						{#if r.linkCopied}
							<Check class="h-3.5 w-3.5 text-green-600" />
							<span class="text-green-600">Copied</span>
						{:else}
							<Copy class="h-3.5 w-3.5" />
							Copy
						{/if}
					</button>
				</div>
				<p class="mt-1.5 text-xs text-slate-400">The link contains your certificate — the supporter needs nothing else.</p>
			</div>

			<div class="card-pad-sm">
				<p class="section-label mb-3">Summary</p>
				<dl class="space-y-0">
					<div class="flex items-center justify-between border-b border-slate-50 py-2 text-sm">
						<dt class="text-slate-500">SA Mail</dt>
						<dd class="font-mono text-xs text-slate-800">{r.saEmail}</dd>
					</div>
					<div class="flex items-center justify-between border-b border-slate-50 py-2 text-sm">
						<dt class="text-slate-500">Key ID</dt>
						<dd class="font-mono text-xs text-slate-800">{r.computedKeyId.slice(0, 16)}…</dd>
					</div>
					<div class="flex items-center justify-between border-b border-slate-50 py-2 text-sm">
						<dt class="text-slate-500">Valid from</dt>
						<dd class="text-slate-800">{r.validFrom}</dd>
					</div>
					<div class="flex items-center justify-between py-2 text-sm">
						<dt class="text-slate-500">Valid to</dt>
						<dd class="text-slate-800">{r.validTo}</dd>
					</div>
				</dl>
				<div class="warning-box">
					<TriangleAlert class="mt-0.5 h-4 w-4 flex-shrink-0 text-amber-500" />
					<p class="text-xs leading-relaxed text-amber-800">
						<strong>Keep this file secure.</strong> It grants access to your service account.
						Document Key ID <code class="rounded bg-amber-100 px-1 py-0.5 font-mono">{r.computedKeyId.slice(0, 8)}…</code>
						in ticket <strong>{r.jiraTicket}</strong>.
					</p>
				</div>
			</div>
		{/if}

	</div>
</div>
```

---

### `webapp/src/routes/upload/+page.svelte`

Replace the entire file (new 3-step flow — no hand-back, step 3 is Done):

```svelte
<script lang="ts">
	import { onMount } from 'svelte';
	import { Globe, ChevronRight, Terminal, Check, Loader2, Copy, CircleCheck } from '@lucide/svelte';
	import TopBar from '$lib/components/TopBar.svelte';
	import KeysTable from '$lib/components/KeysTable.svelte';
	import { createUploadState } from './logic.svelte';

	const STEPS = [{ label: 'Review' }, { label: 'Activate' }, { label: 'Done' }];
	const u = createUploadState();

	onMount(() => u.parseFromUrl());
</script>

<svelte:head>
	<title>SAKE — Activate Certificate</title>
</svelte:head>

<div class="page">
	<TopBar title="Activate Certificate for Requester" steps={STEPS} currentStep={u.displayStep} />

	<div class="content-wrap">

		{#if u.currentStep === '1'}
			<div class="card-pad mb-3">
				<p class="section-label mb-3">Request Details</p>
				<dl class="space-y-0">
					<div class="detail-row-top">
						<dt class="w-28 flex-shrink-0 text-sm text-slate-500">SA Mail</dt>
						<dd class="break-all text-right font-mono text-xs text-slate-800">{u.saEmail}</dd>
					</div>
					<div class="detail-row">
						<dt class="text-sm text-slate-500">Project</dt>
						<dd class="text-sm font-medium text-slate-800">{u.parsedProject}</dd>
					</div>
					<div class="detail-row">
						<dt class="text-sm text-slate-500">Jira Ticket</dt>
						<dd><span class="badge-blue">{u.jiraTicket}</span></dd>
					</div>
					<div class="flex items-center justify-between py-2.5">
						<dt class="text-sm text-slate-500">Expiry</dt>
						<dd class="flex items-center gap-1.5 text-sm text-slate-800">
							{u.validFrom} — {u.validTo}
							<span class="badge-green">{u.expiryDays} days</span>
						</dd>
					</div>
				</dl>
			</div>

			<div class="card-pad-sm">
				<p class="mb-4 text-sm font-medium text-slate-700">Upload this certificate to GCP. Choose how:</p>
				<button onclick={u.signInWithGoogle}
					class="btn-option mb-3 hover:border-blue-400">
					<div class="flex h-9 w-9 flex-shrink-0 items-center justify-center rounded-xl bg-blue-50">
						<Globe class="h-5 w-5 text-blue-600" />
					</div>
					<div class="min-w-0 flex-1">
						<p class="text-sm font-semibold text-slate-900">Login with Google</p>
						<p class="mt-0.5 text-xs text-slate-500">Page handles everything — no terminal needed</p>
					</div>
					<ChevronRight class="h-4 w-4 flex-shrink-0 text-slate-300 transition-colors group-hover:text-blue-500" />
				</button>
				<div class="mb-3 flex items-center gap-3">
					<div class="h-px flex-1 bg-slate-100"></div>
					<span class="text-xs text-slate-400">or</span>
					<div class="h-px flex-1 bg-slate-100"></div>
				</div>
				<button onclick={() => u.goToStep('2b')}
					class="btn-option hover:border-slate-400">
					<div class="flex h-9 w-9 flex-shrink-0 items-center justify-center rounded-xl bg-slate-100">
						<Terminal class="h-5 w-5 text-slate-600" />
					</div>
					<div class="min-w-0 flex-1">
						<p class="text-sm font-semibold text-slate-900">Use the CLI instead</p>
						<p class="mt-0.5 text-xs text-slate-500">
							Get a ready-to-run <code class="font-mono text-slate-600">gcloud</code> command
						</p>
					</div>
					<ChevronRight class="h-4 w-4 flex-shrink-0 text-slate-300 transition-colors group-hover:text-slate-500" />
				</button>
			</div>
		{/if}

		{#if u.currentStep === '2a'}
			<div class="card-pad-sm mb-3">
				<p class="section-label mb-3">Signed in as</p>
				<div class="flex items-center gap-3">
					<div class="avatar">{u.signedInEmail.charAt(0).toUpperCase()}</div>
					<div class="min-w-0 flex-1">
						<p class="text-sm font-semibold text-slate-900">{u.signedInEmail}</p>
						<p class="mt-0.5 flex items-center gap-1 text-xs font-medium text-green-600">
							<Check class="h-3 w-3" /> Authenticated
						</p>
					</div>
					<button onclick={u.switchAccount} class="text-xs text-slate-400 transition-colors hover:text-slate-600">Switch</button>
				</div>
			</div>
			<div class="card-pad-sm mb-3">
				<p class="section-label mb-3">Existing Keys on {u.saEmail.slice(0, 40)}…</p>
				<KeysTable keys={u.existingKeys} onDelete={u.deleteKey} />
				<p class="mt-2 text-xs text-slate-400">Delete expired keys before uploading the new certificate.</p>
			</div>
			<div class="card-pad-sm">
				<button onclick={u.browserUpload} disabled={u.uploading} class="btn-primary">
					{#if u.uploading}
						<Loader2 class="h-4 w-4 animate-spin" /> Uploading…
					{:else}
						<Check class="h-4 w-4" /> Upload &amp; Activate Certificate
					{/if}
				</button>
				<p class="hint">GCP registers the certificate and activates the key.</p>
			</div>
		{/if}

		{#if u.currentStep === '2b'}
			<div class="card-pad-sm mb-3">
				<p class="section-label mb-1">Run in your terminal</p>
				<p class="mb-3 text-xs text-slate-500">Uploads the certificate and activates the key in GCP.</p>
				<div class="relative">
					<pre class="overflow-x-auto whitespace-pre-wrap break-all rounded-xl bg-slate-900 p-4 font-mono text-xs leading-relaxed text-slate-200">{u.cliCommand}</pre>
					<button onclick={u.copyCLI}
						class="absolute right-3 top-3 flex items-center gap-1.5 rounded-lg px-3 py-1.5 text-xs font-medium transition-colors {u.cliCopied ? 'bg-green-700 text-white' : 'bg-slate-700 text-slate-200 hover:bg-slate-600'}">
						{#if u.cliCopied}
							<Check class="h-3.5 w-3.5" /> Copied
						{:else}
							<Copy class="h-3.5 w-3.5" /> Copy
						{/if}
					</button>
				</div>
			</div>
			<div class="card-pad-sm">
				<button onclick={() => u.goToStep('3')} class="btn-primary">
					<Check class="h-4 w-4" /> Command ran successfully
				</button>
			</div>
		{/if}

		{#if u.currentStep === '3'}
			<div class="card-pad mb-3 text-center">
				<div class="mx-auto mb-4 flex h-14 w-14 items-center justify-center rounded-2xl bg-green-100">
					<CircleCheck class="h-7 w-7 text-green-600" />
				</div>
				<h2 class="mb-1 text-lg font-bold text-slate-900">Certificate activated</h2>
				<p class="text-sm leading-relaxed text-slate-500">
					The requester already has their Service Account Key —<br />no need to send anything back.
				</p>
			</div>
			<div class="card-pad-sm">
				<p class="section-label mb-3">For your records</p>
				<dl class="space-y-0">
					<div class="detail-row-top">
						<dt class="w-28 flex-shrink-0 text-sm text-slate-500">SA Mail</dt>
						<dd class="break-all text-right font-mono text-xs text-slate-800">{u.saEmail}</dd>
					</div>
					<div class="detail-row">
						<dt class="text-sm text-slate-500">Jira Ticket</dt>
						<dd><span class="badge-blue">{u.jiraTicket}</span></dd>
					</div>
					<div class="detail-row">
						<dt class="text-sm text-slate-500">Activated</dt>
						<dd class="text-sm text-slate-800">{u.validFrom}</dd>
					</div>
					<div class="flex items-center justify-between py-2.5">
						<dt class="text-sm text-slate-500">Expires</dt>
						<dd class="text-sm text-slate-800">{u.validTo}</dd>
					</div>
				</dl>
				<p class="mt-4 text-center text-xs text-slate-400">You may close this tab. Your work here is done.</p>
			</div>
		{/if}

	</div>
</div>
```

---

## Step 5 — Install & verify ⏳ TODO

```bash
cd webapp
pnpm install --no-frozen-lockfile
pnpm dev
```

Check:
- [ ] `pnpm check` — zero TypeScript errors
- [ ] Landing page renders with Tailwind styles and Inter font
- [ ] `/request/` — form renders, generates cert, SAK downloads
- [ ] `/upload/` — review screen shows cert details, browser login works, CLI command shown
- [ ] Step indicators advance correctly on both routes
