<template>
  <div class="grid gap-3 sm:grid-cols-2">
    <Button
      v-for="(suggestion, index) in suggestions"
      :key="suggestion.suggestion_id"
      class="h-auto min-h-16 justify-start gap-3 whitespace-normal py-3 text-left text-base font-semibold"
      :disabled="disabled"
      :variant="
        selectedSuggestionId === suggestion.suggestion_id
          ? 'default'
          : 'outline'
      "
      @click="emit('select', suggestion.suggestion_id)"
    >
      <span
        class="bg-background/20 flex size-8 shrink-0 items-center justify-center rounded-md text-sm font-bold"
        aria-hidden="true"
      >
        {{ optionLetter(index) }}
      </span>
      <div class="flex min-w-0 flex-1 items-center gap-3">
        <img
          v-if="suggestion.image_url"
          :src="getMediaImageUrl(suggestion.image_url)"
          :alt="suggestion.label"
          class="size-16 shrink-0 rounded-md object-cover"
        />

        <span
          class="min-w-0 flex-1 break-words whitespace-pre-line leading-snug"
        >
          {{ getSuggestionDisplayLabel(suggestion.label) }}
        </span>
      </div>
    </Button>
  </div>
</template>

<script setup lang="ts">
import { Button } from "@/components/ui/button";
import type { MusicQuizSuggestion } from "@/composables/music-quiz/useMusicQuiz";
import { getMediaImageUrl } from "@/helpers/utils";

const OPTION_LETTERS = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

defineProps<{
  suggestions: MusicQuizSuggestion[];
  disabled: boolean;
  selectedSuggestionId: string | null;
}>();
const emit = defineEmits<{ select: [suggestionId: string] }>();

function optionLetter(index: number): string {
  return OPTION_LETTERS[index] ?? String(index + 1);
}

function getSuggestionDisplayLabel(label: string): string {
  return label
    .replace(/\s*\([^)]*\)/g, "")
    .replace(/[ \t]{2,}/g, " ")
    .trim();
}
</script>
