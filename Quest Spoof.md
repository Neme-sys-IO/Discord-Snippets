# Quest Spoof
<div style="border: 2px solid #a3d9a5; border-radius: 8px; padding: 16px; background-color: #e8f5e9; color: #2e7d32; font-family: Arial, sans-serif;">
  <strong style="font-size: 18px;">WORKING</strong>
</div>


# DESCRIPTION
This snippet will spoof your stream to fulfill the requirements of the quest and monitor your progress until completion.
> [!NOTE]
> Only works for `STREAM` type of quest.!
# USAGE
1. Accept a Quest.
2. Join a VC. (If doesn't work, join with alt/friend)
3. Start streaming anything.
4. Open console and run the code.
5. Wait for it and Done! Yay.
# CODE
```js
let wpRequire;
window.webpackChunkdiscord_app.push([[Math.random()], {}, (req) => { wpRequire = req; }]);
const cache = wpRequire.c;
const findStore = (key) => Object.values(cache).find(x => x?.exports?.[key]);
const ApplicationStreamingStore = findStore('Z');
const RunningGameStore = findStore('ZP');
const QuestsStore = findStore('Z');
const ExperimentStore = findStore('Z');
const FluxDispatcher = findStore('Z');
const api = findStore('tn');
const quest = [...QuestsStore.quests.values()].find(q => 
    q.id !== "1248385850622869556" && 
    q.userStatus?.enrolledAt && 
    !q.userStatus?.completedAt && 
    new Date(q.config.expiresAt).getTime() > Date.now()
);
const isApp = navigator.userAgent.includes("Electron/");

if (!isApp) {
    console.log("%cNot available in the browser.", "font-size: 16px; color: red");
} else if (!quest) {
    console.log("%cNo active quests found.", "font-size: 16px; color: green");
} else {
    const pid = Math.floor(Math.random() * 30000) + 1000;
    let applicationId, applicationName, secondsNeeded, secondsDone, canPlay;

    if (quest.config.configVersion === 1) {
        ({ applicationId, applicationName } = quest.config);
        secondsNeeded = quest.config.streamDurationRequirementMinutes * 60;
        secondsDone = quest.userStatus?.streamProgressSeconds ?? 0;
        canPlay = quest.config.variants.includes(2);
    } else if (quest.config.configVersion === 2) {
        ({ id: applicationId, name: applicationName } = quest.config.application);
        canPlay = ExperimentStore.getUserExperimentBucket("2024-04_quest_playtime_task") > 0 && quest.config.taskConfig.tasks["PLAY_ON_DESKTOP"];
        const taskName = canPlay ? "PLAY_ON_DESKTOP" : "STREAM_ON_DESKTOP";
        secondsNeeded = quest.config.taskConfig.tasks[taskName].target;
        secondsDone = quest.userStatus?.progress?.[taskName]?.value ?? 0;
    }

    const handleGameSpoofing = (appData) => {
        const exeName = appData.executables.find(x => x.os === "win32").name.replace(">", "");
        const games = RunningGameStore.getRunningGames();
        const fakeGame = {
            cmdLine: C:\\Program Files\\${appData.name}\\${exeName},
            exeName,
            exePath: c:/program files/${appData.name.toLowerCase()}/${exeName},
            hidden: false,
            isLauncher: false,
            id: applicationId,
            name: appData.name,
            pid,
            pidPath: [pid],
            processName: appData.name,
            start: Date.now(),
        };
        games.push(fakeGame);
        FluxDispatcher.dispatch({ type: "RUNNING_GAMES_CHANGE", removed: [], added: [fakeGame], games });

        const progressHandler = (data) => {
            const progress = quest.config.configVersion === 1 
                ? data.userStatus.streamProgressSeconds 
                : Math.floor(data.userStatus.progress.PLAY_ON_DESKTOP.value);

            console.log(`%cProgress: ${progress}/${secondsNeeded}`, "font-size: 14px; color: blue");
            if (progress >= secondsNeeded) {
                console.log("%cQuest completed.", "font-size: 14px; color: purple");
                games.splice(games.indexOf(fakeGame), 1);
                FluxDispatcher.dispatch({ type: "RUNNING_GAMES_CHANGE", removed: [fakeGame], added: [], games });
                FluxDispatcher.unsubscribe("QUESTS_SEND_HEARTBEAT_SUCCESS", progressHandler);
            }
        };

        FluxDispatcher.subscribe("QUESTS_SEND_HEARTBEAT_SUCCESS", progressHandler);
        console.log(`%cGame spoofed: ${applicationName}. ${Math.ceil((secondsNeeded - secondsDone) / 60)} minutes remaining.`, "font-size: 14px; color: orange");
    };

    const handleStreamSpoofing = () => {
        const originalFunc = ApplicationStreamingStore.getStreamerActiveStreamMetadata;
        ApplicationStreamingStore.getStreamerActiveStreamMetadata = () => ({ id: applicationId, pid, sourceName: null });

        const progressHandler = (data) => {
            const progress = quest.config.configVersion === 1 
                ? data.userStatus.streamProgressSeconds 
                : Math.floor(data.userStatus.progress.STREAM_ON_DESKTOP.value);

            console.log(`%cProgress: ${progress}/${secondsNeeded}`, "font-size: 14px; color: blue");
            if (progress >= secondsNeeded) {
                console.log("%cQuest completed.", "font-size: 14px; color: purple");
                ApplicationStreamingStore.getStreamerActiveStreamMetadata = originalFunc;
                FluxDispatcher.unsubscribe("QUESTS_SEND_HEARTBEAT_SUCCESS", progressHandler);
            }
        };

        FluxDispatcher.subscribe("QUESTS_SEND_HEARTBEAT_SUCCESS", progressHandler);
        console.log(`%cStreaming spoofed: ${applicationName}. ${Math.ceil((secondsNeeded - secondsDone) / 60)} minutes remaining.`, "font-size: 14px; color: orange");
    };

    if (canPlay) {
        api.get({ url: /applications/public?application_ids=${applicationId} }).then(res => handleGameSpoofing(res.body[0]));
    } else {
        handleStreamSpoofing();
    }
}
```
