# openclaw-eval-group2
组长：黄奕曦 组员：杨文 丘仕军
第一周
仓库创建，websocket连接两台openclaw，尝试压力测试,为后续测试打下基础（可以通过大龙虾操作小龙虾，小龙虾作为一个node接入大龙虾）
连接两台openclaw所用代码如下：
pkill -f client.js

const WebSocket = require('ws');
const wss = new WebSocket.Server({ host: '0.0.0.0', port: 8080 });

console.log('🚀 性能测评服务端启动中...');

let totalReceived = 0;
let startTime = Date.now();

// 每 5 秒报告一次当前系统状态（模拟监控看板）
setInterval(() => {
    const uptime = ((Date.now() - startTime) / 1000).toFixed(0);
    const memoryUsage = (process.memoryUsage().rss / 1024 / 1024).toFixed(2);
    console.log(`[监控] 运行时间: ${uptime}s | 总消息数: ${totalReceived} | 内存占用: ${memoryUsage} MB`);
}, 5000);

wss.on('connection', (ws) => {
    console.log('🦞 压测客户端已接入');

    ws.on('message', (data) => {
        totalReceived++;
        // 原样返回，方便客户端计算 RTT（往返延迟）
        ws.send(data); 
    });

    ws.on('close', () => console.log('🔌 连接已断开'));
});
//client大龙虾
//server小龙虾
