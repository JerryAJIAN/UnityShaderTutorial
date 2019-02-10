# Abstract

���ǽ� ���̴��� �̿��Ͽ� ���������� ��µǴ� �÷����� �����غ���

# Shader

```c
Shader "UnityShaderTutorial/surface_shader_final_color_mod" {
	Properties{
		_MainTex("Texture", 2D) = "white" {}
		_ColorTint("Tint", Color) = (1.0, 0.6, 0.6, 1.0)
	}

	SubShader{
		Tags{ "RenderType" = "Opaque" }
		CGPROGRAM
#pragma surface surf Lambert finalcolor:mycolor
		struct Input {
			float2 uv_MainTex;
		};

		fixed4 _ColorTint;
		void mycolor(Input IN, SurfaceOutput o, inout fixed4 color) {
			color *= _ColorTint;
		}

		sampler2D _MainTex;
		void surf(Input IN, inout SurfaceOutput o) {
			o.Albedo = tex2D(_MainTex, IN.uv_MainTex).rgb;
		}
		ENDCG
	}
		Fallback "Diffuse"
}
```

# Description

�� ���̴��� ���������� ȭ�鿡 ��µ� �� ��ü�� ������ �����ϴ� ���� �÷� ������̾�(final color modifier) ����� ����Ѵ�.

surface shader �� �ɼ����� `finalcolor:ColorFunction`�� ����Ѵ�. �ش� �ɼ��� `ColorFunction`�� ������ �Լ��� fragment shader �� ������ �ܰ迡 �߰��Ѵ�.

```
fixed4 frag_surf (v2f_surf IN) : SV_Target {
  // realtime lighting: call lighting function
  c += LightingLambert (o, gi);
  mycolor (surfIN, o, c);
  UNITY_OPAQUE_ALPHA(c.a);
  return c;
}
```

�߰�������, `finalcolor` �ɼ��� ����ϸ� fragment shader�� ������ �ܰ迡�� �����ϴ� `UNITY_APPLY_FOG(IN.fogCoord, c)` �Լ��� ���ǵ��� �ʴ´�.