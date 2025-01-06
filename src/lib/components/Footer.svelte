<script lang="ts">
    import { Footer, FooterCopyright, FooterLinkGroup, FooterLink, FooterBrand, FooterIcon } from 'flowbite-svelte';
    import { FacebookSolid, GithubSolid, DiscordSolid, TwitterSolid } from 'flowbite-svelte-icons';
    let year = $state(new Date().getFullYear())

    const company = {name: 'NosCorp', logo: 'https://w7.pngwing.com/pngs/985/67/png-transparent-blue-logo-circle-logo-symbol-font-templates-blue-angle-text-thumbnail.png'}
</script>
  
<Footer footerType="socialmedia">
    <div class="md:flex md:justify-between">
        <div class="mb-6 md:mb-0">
            <FooterBrand href="/" src={company.logo} alt={company.name} name={company.name} />
        </div>
        <div class="grid grid-cols-2 gap-8 sm:gap-6 sm:grid-cols-3">
        <div>
            <h2 class="mb-6 text-sm font-semibold text-gray-900 uppercase dark:text-white">Home</h2>
            <FooterLinkGroup>
                <FooterLink liClass="mb-4" href="/">Flowbite</FooterLink>
            </FooterLinkGroup>
        </div>
        <div>
            <h2 class="mb-6 text-sm font-semibold uppercase text-gray-900 dark:text-white">Portfolio</h2>
            <FooterLinkGroup>
                <FooterLink liClass="mb-4" href="/">GitHub</FooterLink>
            </FooterLinkGroup>
        </div>
        <div>
            <h2 class="mb-6 text-sm font-semibold uppercase text-gray-900 dark:text-white">About</h2>
            <FooterLinkGroup>
                <FooterLink liClass="mb-4" href="/">Privacy Policy</FooterLink>
            </FooterLinkGroup>
        </div>
        </div>
    </div>

    <hr class="my-6 border-gray-200 sm:mx-auto dark:border-gray-700 lg:my-8" />
    
    <div class="sm:flex sm:items-center sm:justify-between">
        <FooterCopyright href="/" by={company.name} />
        <div class="flex mt-4 space-x-6 rtl:space-x-reverse sm:justify-center sm:mt-0">
            <FooterIcon href="/">
                <FacebookSolid class="w-5 h-5 text-gray-500 dark:text-gray-500 hover:text-gray-900 dark:hover:text-white" />
            </FooterIcon>
            <FooterIcon href="/">
                <DiscordSolid class="w-5 h-5 text-gray-500 dark:text-gray-500 hover:text-gray-900 dark:hover:text-white" />
            </FooterIcon>
            <FooterIcon href="/">
                <TwitterSolid class="w-5 h-5 text-gray-500 dark:text-gray-500 hover:text-gray-900 dark:hover:text-white" />
            </FooterIcon>
            <FooterIcon href="/">
                <GithubSolid class="w-5 h-5 text-gray-500 dark:text-gray-500 hover:text-gray-900 dark:hover:text-white" />
            </FooterIcon>
        </div>
    </div>
</Footer>