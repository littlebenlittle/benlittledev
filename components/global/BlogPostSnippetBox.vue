<template>
    <CFlex
        direction="row"
        align="left"
    >
        <CFlex
            mx="4"
            width="100px"
        >
            <CFlex
                mx="4"
                align-items="center"
                direction="row"
            >
                <CButton
                    @click="$emit('select')"
                    variant-color="blue"
                    :variant="selected ? 'solid' : 'outline'"
                >
                  view
                </CButton>
            </CFlex>
        </CFlex>
        <CFlex
            direction="column"
            align-items="left"
            justify-content="space-between"
            :width="width"
        >
            <CHeading
                fontSize="xl"
                :as="titleHeading"
                padding="2"
            >
                {{ title  }}
            </CHeading>
            <CFlex
                direction="row"
                justify="left"
            >
                <CBox
                >
                    <CHeading
                        fontSize="md"
                        :as="dateHeading"
                        padding="2"
                    >
                        {{ date | fmtDate }}
                    </CHeading>
                </CBox>
            </CFlex>
        </CFlex>
    </CFlex>
</template>

<script>
import moment from "moment"
import {
	CBox, CFlex, CHeading, CText, CButton, CCollapse,
	CSimpleGrid,
} from "@chakra-ui/vue"

function headingValidator(val) {
	return [ "h1", "h2", "h3", "h4", "h5", "h6" ].indexOf(val) != -1
}

export default {
	name: "BlogPostSnippetBox",

	components: {
		CBox, CFlex, CHeading, CText, CButton, CCollapse,
		CSimpleGrid,
	},

	model: {
		prop: 'selected',
		event: 'change',
	},

	props: {
		id: {
			type: String,
		},
		title: {
			type: String,
		},
		date: {
			type: String,
		},
		selected: {
			type: Boolean,
		},
		titleHeading: {
			default: "h3",
			validator: headingValidator,
		},
		dateHeading: {
			default: "h4",
			validator: headingValidator,
		},
		width: {
			type: String,
			default: "",
		},
	},

	filters: {
		fmtDate(val) {
			return moment(val).add(1, 'day').format('MMM Do, YYYY')
		},
	},

}
</script>

<style>
</style>
