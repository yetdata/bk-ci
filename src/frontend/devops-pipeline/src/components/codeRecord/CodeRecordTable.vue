<template>
    <div class="code-record-table-container" v-if="commitList.length">
        <p class="prompt-tips">
            {{ $t('details.commitIdsRange') }}
            <span v-if="commitList.length > 1">
                <span>{{ formatCommitId(lastCommit.commit) }}</span>
                <span>-</span>
                <span>{{ formatCommitId(firstCommit.commit) }}</span>
            </span>
            <span v-else>
                <span>{{ formatCommitId(firstCommit.commit) }}</span>
            </span>
        </p>
        <bk-table :data="commitList">
            <bk-table-column :label="$t('history.remark')" prop="comment"></bk-table-column>
            <bk-table-column
                :width="150"
                label="Commit"
                prop="commit"
                :formater="row => formatCommitId(row.commit)"
            ></bk-table-column>
            <bk-table-column :width="150" :label="$t('details.committer')" prop="committer"></bk-table-column>
            <bk-table-column
                :width="200"
                :label="$t('details.commitTime')"
                prop="commitTime"
                :formatter="formatTimeColumn"
            ></bk-table-column>
        </bk-table>
    </div>
    <div v-else class="code-records-empty">
        <div class="no-data-right">
            <img src="../../images/box.png">
            <p>
                <span>{{ label }}</span>{{ $t('details.noCodeRecords') }}
            </p>
        </div>
    </div>
</template>

<script>
    import { convertTime } from '@/utils/util'
    export default {
        name: 'code-record-table',
        props: {
            commitList: {
                type: Array,
                default: []
            },
            label: {
                type: String,
                default: ''
            }
        },
        computed: {
            hasCommitList () {
                return Array.isArray(this.commitList) && this.commitList.length > 0
            },
            firstCommit () {
                return this.hasCommitList ? this.commitList[0] : ''
            },
            lastCommit () {
                const len = this.commitList.length
                return this.hasCommitList && len > 0 ? this.commitList[len - 1] : ''
            }
        },
        methods: {
            formatTimeColumn (row) {
                return row.commitTime ? convertTime(row.commitTime) : '--'
            },
            formatCommitId (commitId) {
                return commitId && typeof commitId === 'string' ? commitId.slice(0, 8) : '--'
            }
        }
    }
</script>

<style lang="scss">
    .code-record-table-container {
        overflow: auto;
        height: 100%;
        .prompt-tips {
            margin-bottom: 12px;
        }
    }
</style>
