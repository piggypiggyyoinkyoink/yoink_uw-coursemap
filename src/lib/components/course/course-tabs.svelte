<script lang="ts">
    import type {Course} from "$lib/types/course.js";
    import {Tabs, TabsContent, TabsList, TabsTrigger} from "$lib/components/ui/tabs/index.js";
    import type {Terms} from "$lib/types/terms.js";
    import {type FullInstructorInformation, getFullInstructorInformation} from "$lib/types/instructor.ts";
    import CourseDetails from "./course-details.svelte";
    import {CourseOverview} from "$lib/components/course/tabs";
    import {CourseInstructors, CoursePrerequisites, CourseTrends} from "$lib/components/course/tabs/index.js";
    import type {ElementDefinition} from "cytoscape";
    import type {StyleEntry} from "$lib/components/cytoscape/graph-styles.ts";

    interface Props {
        course: Course;
        similarCourses: Course[];
        terms: Terms;
        selectedTerm: string | undefined;
        instructors: FullInstructorInformation[],
        prerequisiteElementDefinitions: ElementDefinition[],
        prerequisiteStyleEntries: StyleEntry[],
    }

    let {
        course,
        similarCourses,
        terms,
        selectedTerm,
        instructors,
        prerequisiteElementDefinitions,
        prerequisiteStyleEntries,
    }: Props = $props();

</script>

<Tabs value="overview">
    <TabsList class="my-2">
        <TabsTrigger value="overview">Overview</TabsTrigger>
        <TabsTrigger value="trends">Trends</TabsTrigger>
        <TabsTrigger value="instructors">Instructors</TabsTrigger>
        <TabsTrigger value="prerequisites">Prerequisites Map</TabsTrigger>
    </TabsList>
    <div class="grid gap-4 lg:grid-cols-12">
        <CourseDetails {course} {selectedTerm}/>
        <TabsContent class="lg:col-span-9 space-y-4" value="overview">
            <CourseOverview {course} {similarCourses} {instructors} {selectedTerm} {terms}/>
        </TabsContent>
        <TabsContent class="lg:col-span-9 space-y-4" value="trends">
            <CourseTrends {course} {terms}/>
        </TabsContent>
        <TabsContent class="lg:col-span-9 space-y-4" value="instructors">
            <CourseInstructors {instructors}/>
        </TabsContent>
        <TabsContent class="lg:col-span-9 space-y-4" value="prerequisites">
            <CoursePrerequisites {course} {prerequisiteElementDefinitions} {prerequisiteStyleEntries}/>
        </TabsContent>
    </div>
</Tabs>