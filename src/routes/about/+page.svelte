<script lang="ts">
	import Title from "$lib/components/Title.svelte"
    import { resume } from "$lib/types"
</script>

<Title title="About" header="About Me"/>

<h3>{resume.name.toUpperCase()}</h3> <hr/>
<div>
    <p>
        {resume.contact.phone} - 
        {resume.contact.email} - 
        <a href={resume.contact.website}>{resume.contact.website}</a> - 
        {resume.contact.address}
    </p>
</div>

<div><h4>SUMMARY:</h4></div>
<div>
    <p>{resume.summary}</p>
</div>

<h4>SKILLS:</h4>
<div>
    <ul>
        {#each resume.skills as skill}
            <li>{skill}</li>
        {/each}
    </ul>
</div>

<h4>EXPERIENCE:</h4>
<div>
    {#each resume.experience as experience}
        <div><b>{experience.company.startDate}</b> - {experience.company.endDate}</div>
        <div>{experience.company.title}, {experience.company.name}, {experience.company.location}</div>
        <ul>
            {#each experience.company.tasks as task}
                <li>{task}</li>
            {/each}
        </ul>
    {/each}
</div>

<h4>EDUCATION:</h4>
<div>
    {#each resume.education as education}
        <div>{education.intitute}</div>
        <div>{education.kind} - {education.title}</div>
        <div>{education.location} - {education.conclusion}</div>
    {/each}
</div>


<h4>CERTIFICATION:</h4>
<div>
    <ul>
        {#each resume.certification as certification}
            <li>{certification.title} - {certification.provider} - {certification.conclusion}</li>    
        {/each}
    </ul>
</div>

<style>
    h3 {
        color: rgb(97, 149, 245);
        text-align: center;
    }

    h4 {
        color: rgb(97, 149, 245);
        border-style: dashed;
        border-top: 0;
        border-left: 0;
        border-right: 0;
        border-bottom: 0,5;
    }

    div {
        border: 0.5;
    }

    li {
        list-style-type: disc;
    }
</style>