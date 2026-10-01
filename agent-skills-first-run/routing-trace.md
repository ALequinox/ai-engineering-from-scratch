{
  "activated": true,
  "actor": "human",
  "adapter": "host-extension-policy",
  "channel": "explicit-human",
  "reason": "exact human selection",
  "score": 1.0,
  "skill": "my-first-skill"
}
{
  "activated": true,
  "actor": "model",
  "adapter": "host-extension-policy",
  "channel": "implicit-model",
  "reason": "model relevance threshold",
  "score": 0.3,
  "skill": "my-first-skill"
}
{
  "activated": false,
  "actor": "model",
  "adapter": "host-extension-policy",
  "channel": "implicit-model",
  "reason": "relevance threshold blocked activation",
  "score": 0.0,
  "skill": "my-first-skill"
}
{
  "activated": false,
  "actor": "model",
  "adapter": "host-extension-policy",
  "channel": "implicit-model",
  "reason": "relevance threshold blocked activation",
  "score": 0.0417,
  "skill": "my-first-skill"
}

my-first-skill 可以由用户手动调用和在合适场景下由大模型调用，activated: true 表面该skill在该场景下可以被调用，并不能代表已加载技能正文并批准了工具操作