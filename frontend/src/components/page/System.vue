<template>
    <div>
        <el-row type="flex" justify="space-around" class="row-section">
             <el-col :span="22">
                <el-card>
                    <div slot="header" align="center">
                        <h3 class="title-tag">
                            <el-tag type="success">System Charts</el-tag>
                        </h3>
                    </div>
                    <el-row type="flex" justify="space-around">
                        <el-col :span="11">
                            <div class="charts" v-if="load_charts.names.length">
                                <time-line-charts :values="load_charts.data" :names="load_charts.names"
                                                  :title="load_charts.title"></time-line-charts>
                            </div>
                        </el-col>

                        <el-col :span="11">
                            <div class="charts" v-if="cpu_charts.names.length">
                                <bar-charts :values="cpu_charts.data" :names="cpu_charts.names" :title="cpu_charts.title">
                                </bar-charts>
                            </div>
                        </el-col>
                    </el-row>
                </el-card>
             </el-col>
        </el-row>

        <el-row type="flex" justify="space-around" class="row-section">
            <el-col :span="22">
                <el-card>
                    <div slot="header" align="center">
                        <h3 class="title-tag">
                            <el-tag type="success">Cpu Usage</el-tag>
                        </h3>
                    </div>
                    <el-row type="flex" justify="space-around">
                        <el-table :data="cpus" border
                                  :defaultSort="{prop: 'name', order: 'ascending'}">
                            <el-table-column prop="name" label="name" sortable align="center"></el-table-column>
                            <el-table-column prop="total" label="total" sortable align="center"></el-table-column>
                            <el-table-column prop="system" label="system" sortable align="center"></el-table-column>
                            <el-table-column prop="user" label="user" sortable align="center"></el-table-column>
                            <el-table-column prop="idle" label="idle" sortable align="center"></el-table-column>
                            <el-table-column prop="nice" label="nice" sortable align="center"></el-table-column>
                            <el-table-column prop="iowait" label="iowait" sortable align="center"></el-table-column>
                            <el-table-column prop="steal" label="steal" sortable align="center"></el-table-column>
                            <el-table-column prop="irq" label="irq" sortable align="center"></el-table-column>
                            <el-table-column prop="soft_irq" label="soft_irq" sortable align="center"></el-table-column>
                        </el-table>
                    </el-row>
                </el-card>
            </el-col>
        </el-row>

        <el-row type="flex" justify="space-around" class="row-section">
            <el-col :span="22">
                <el-card>
                    <div slot="header" align="center">
                        <h3 class="title-tag">
                            <el-tag type="success">Mem Usage</el-tag>
                        </h3>
                    </div>

                    <el-row type="flex" justify="space-around">
                        <el-col :span="11">
                            <div class="charts" v-if="mem_percent.names.length">
                                <time-line-charts :values="mem_percent.data" :label="mem_percent.label"
                                                  :names="mem_percent.names" :title="mem_percent.title">
                                </time-line-charts>
                            </div>
                        </el-col>

                        <el-col :span="11">
                            <div class="charts" v-if="mem_usage.names.length">
                                <bar-charts :values="mem_usage.data" :names="mem_usage.names"
                                            :title="mem_usage.title" :label="mem_usage.label">
                                </bar-charts>
                            </div>
                        </el-col>
                    </el-row>
                </el-card>
            </el-col>
        </el-row>
        <el-row type="flex" justify="space-around" class="row-section">
            <el-col :span="22">
                <el-card>
                    <el-row type="flex" justify="space-around">

                        <el-col :span="11">
                            <el-card>
                                <div  slot="header" align="center">
                                    <h3 class="title-tag">
                                        <el-tag type="success">Cpu Intensive Processes</el-tag>
                                    </h3>
                                </div>
                                <el-row type="flex" justify="space-around">
                                    <el-table :data="cpu_intensive" border>
                                        <el-table-column label="pid" align="center">
                                            <template scope="scope">
                                                <el-button type="text" size="small" :disabled="scope.row.pid == '-'">
                                                    <router-link :to="{name: 'process', params:{pid: scope.row.pid}}" tag="span" v-if="scope.row.pid != '-'">
                                                        {{scope.row.pid}}
                                        </router-link>
                                                    <span v-else>-</span>
                                                </el-button>
                                            </template>
                                        </el-table-column>
                                        <el-table-column label="name" prop="name" align="center">
                                        </el-table-column>
                                        <el-table-column prop="cpu_percent" label="cpu(%)" align="center">
                                        </el-table-column>
                                        <el-table-column prop="status" label="status" align="center"></el-table-column>
                                        <el-table-column prop="memory_percent" label="mem(%)" align="center"></el-table-column>
                                    </el-table>
                                </el-row>
                            </el-card>
                        </el-col>

                        <el-col :span="11">
                            <el-card>
                                <div slot="header" align="center">
                                    <h3 class="title-tag">
                                        <el-tag type="success">Memory Intensive Processes</el-tag>
                                    </h3>
                                </div>
                                <el-row type="flex" justify="space-around">
                                    <el-table :data="mem_intensive" border>
                                        <el-table-column label="pid" align="center">
                                            <template scope="scope">
                                                <el-button type="text" size="small" :disabled="scope.row.pid == '-'">
                                                    <router-link :to="{name: 'process', params:{pid: scope.row.pid}}" tag="span" v-if="scope.row.pid != '-'">
                                                        {{scope.row.pid}}
                                        </router-link>
                                                    <span v-else>-</span>
                                                </el-button>
                                            </template>
                                        </el-table-column>
                                        <el-table-column label="name" prop="name" align="center"></el-table-column>
                                        <el-table-column label="status" prop="status" align="center"></el-table-column>
                                        <el-table-column prop="memory_percent" label="mem(%)" align="center">
                                        </el-table-column>
                                    </el-table>
                                </el-row>
                            </el-card>
                        </el-col>
                    </el-row>
                </el-card>
            </el-col>

        </el-row>

        <el-row type="flex" justify="space-around" class="row-section">
            <el-col :span="22">
                <el-card>
                    <div slot="header" align="center">
                        <h3 class="title-tag">
                            <el-tag type="success">IO Summary</el-tag>
                        </h3>
                    </div>

                    <el-row type="flex" justify="space-around">
                        <el-col :span="11">
                            <div class="charts" v-if="io_bytes.names.length">
                                <time-line-charts :values="io_bytes.data" :names="io_bytes.names"
                                                  :title="io_bytes.title"></time-line-charts>
                            </div>
                        </el-col>

                        <el-col :span="11">
                            <div class="charts" v-if="io_count.names.length">
                                <time-line-charts :values="io_count.data" :names="io_count.names"
                                                  :title="io_count.title"></time-line-charts>
                            </div>
                        </el-col>
                    </el-row>
                </el-card>
            </el-col>
        </el-row>

        <el-row type="flex" justify="space-around" class="row-section">
            <el-col :span="22">
                <el-card>
                    <div slot="header" align="center">
                        <h3 class="title-tag">
                            <el-tag type="success">IO Average</el-tag>
                        </h3>
                    </div>

                    <el-row type="flex" justify="space-around">
                        <el-col :span="11">
                            <div class="charts" v-if="io_avg_bytes.names.length">
                                <time-line-charts :values="io_avg_bytes.data" :names="io_avg_bytes.names"
                                                  :title="io_avg_bytes.title"></time-line-charts>
                            </div>
                        </el-col>

                        <el-col :span="11">
                            <div class="charts" v-if="io_avg_time.names.length">
                                <time-line-charts :values="io_avg_time.data" :names="io_avg_time.names"
                                                  :title="io_avg_time.title"></time-line-charts>
                            </div>
                        </el-col>
                    </el-row>
                </el-card>
            </el-col>
        </el-row>
    </div>
</template>

<script>
    import config from '../../config';
    import TimeLineCharts from '../common/TimeLineCharts.vue';
    import BarCharts from '../common/BarCharts.vue';

    export default {
        components: {
            'time-line-charts': TimeLineCharts,
            'bar-charts': BarCharts
        },

        data: function () {
            return {
                mem_intensive: [],
                cpu_intensive: [],
                cpus: [],
                tick: null,
                load_charts: {
                    title: 'load average',
                    names: [],
                    data: {
                        '1min': [],
                        '3min': [],
                        '5min': []
                    }
                },

                io_bytes: {
                    title: 'read/write bytes',
                    names: [],
                    data: {
                        'read': [],
                        'write': []
                    }
                },

                io_count: {
                    title: 'read/write count',
                    names: [],
                    data: {
                        'read': [],
                        'write': []
                    }
                },

                io_avg_bytes: {
                    title: 'bytes/io',
                    names: [],
                    data: {
                        'read': [],
                        'write': []
                    }
                },

                io_avg_time: {
                    title: 'mil-secs/io',
                    names: [],
                    data: {
                        'read': [],
                        'write': []
                    }
                },

                mem_percent: {
                    title: 'mem percent',
                    names: [],
                    label: {
                        formatter: '{value} %'
                    },

                    data: {
                        'used': []
                    }
                },

                mem_usage: {
                    title: 'mem usage',
                    label: {
                        formatter: '{value} M'
                    },
                    names: [],
                    data: []
                },

                cpu_charts: {
                    title: 'cpu.idle',
                    names: [],
                    data: []
                },

                pstree: [],
                cpustat: {},
                pstree_props: {
                    label: 'process',
                    children: 'children'
                }
            }
        },

        methods: {
            getSystemInfo: function () {
                api.system.get().then((response) => {
                    let _self = this;
                    let sys_info = response.data.data;
                    _self.cpu_intensive = sys_info['intensive_processes'].cpu_intensive;
                    _self.mem_intensive = sys_info['intensive_processes'].mem_intensive;
                    _self.pushToLoadCharts(sys_info['loadavg']);
                    let cpustat = sys_info['cpu_stat'];
                    _self.cpus = cpustat.cpus;
                    _self.pushToCpuCharts(cpustat['cpus']);
                    let mem_info = sys_info.mem_info;
                    _self.pushToMemCharts(mem_info);
                    let io_counters = sys_info.io_counters;
                    _self.pushToIOCharts(io_counters);
                }, (error) => {
                });
            },

            pushToLoadCharts: function (data_point) {
                for(let key in data_point){
                    this.load_charts.data[key].push(data_point[key]);
                }

                const now = new Date();
                const timestamp = [now.getHours(), now.getMinutes() , now.getSeconds()].join(':');
                this.load_charts.names.push(timestamp);
            },

            pushToCpuCharts: function (cpus) {
                this.cpu_charts.names.length = 0;
                this.cpu_charts.data.length = 0;

                for(let cpu of cpus){
                    this.cpu_charts.data.push(cpu['idle']);
                    this.cpu_charts.names.push(cpu['name']);
                }
            },

            pushToMemCharts: function (mem) {
                const now = new Date();
                const timestamp = [now.getHours(), now.getMinutes() , now.getSeconds()].join(':');
                const usage_keys = ['total', 'free', 'available', 'active', 'buffers', 'cached'];
                this.mem_percent.data['used'].push(mem.percent);
                this.mem_percent.names.push(timestamp);

                this.mem_usage.names.length = 0;
                this.mem_usage.data.length = 0;
                for(let key of Object.keys(mem)){
                    if(usage_keys.indexOf(key) != -1){
                        this.mem_usage.data.push({
                           'name': key,
                            'value': mem[key]
                        });

                        this.mem_usage.names.push(key);
                    }
                }
            },

            pushToIOCharts: function (io_counters) {
                const now = new Date();
                const timestamp = [now.getHours(), now.getMinutes() , now.getSeconds()].join(':');

                let io_bytes = this.io_bytes;
                io_bytes.data['read'].push(io_counters['read_bytes']);
                io_bytes.data['write'].push(io_counters['write_bytes']);
                io_bytes.names.push(timestamp);

                let io_count = this.io_count;
                io_count.data['read'].push(io_counters['read_count']);
                io_count.data['write'].push(io_counters['write_count']);
                io_count.names.push(timestamp);

                let io_avg_bytes = this.io_avg_bytes;
                io_avg_bytes.data['read'].push(io_counters['avg_read_bytes']);
                io_avg_bytes.data['write'].push(io_counters['avg_write_bytes']);
                io_avg_bytes.names.push(name);

                let io_avg_time = this.io_avg_time;
                io_avg_time.data['read'].push(io_counters['avg_read_time']);
                io_avg_time.data['write'].push(io_counters['avg_write_time']);
                io_avg_time.names.push(name);
            }
        },

        mounted: function () {
            this.getSystemInfo();
            this.tick = setInterval(this.getSystemInfo, config.SYSTEM_IDLE);
        },

        destroyed: function () {
            if(this.tick){
                clearInterval(this.tick);
            }
        }

    }
</script>
