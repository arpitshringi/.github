<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Arpit Shringi | Portfolio</title>
    <style>
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; margin: 0; padding: 0; background-color: #f4f7f6; color: #333; }
        header { background-color: #2c3e50; color: white; padding: 3rem 2rem; text-align: center; }
        header h1 { margin: 0; font-size: 2.5rem; }
        header p { font-size: 1.2rem; font-weight: 300; margin-top: 10px; color: #ecf0f1; }
        .container { max-width: 900px; margin: 2rem auto; padding: 0 20px; }
        .section-title { text-align: center; font-size: 2rem; margin-bottom: 2rem; color: #2c3e50; }
        .skills-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 20px; }
        .skill-card { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1); transition: transform 0.3s; }
        .skill-card:hover { transform: translateY(-5px); }
        .skill-icon { font-size: 2rem; margin-bottom: 10px; color: #3498db; }
        .skill-title { font-size: 1.2rem; font-weight: bold; margin-bottom: 10px; color: #2c3e50; }
        .skill-desc { font-size: 0.95rem; line-height: 1.5; color: #555; }
        footer { text-align: center; padding: 2rem; background-color: #2c3e50; color: white; margin-top: 2rem; }
        .contact-btn { display: inline-block; margin-top: 15px; padding: 10px 20px; background-color: #3498db; color: white; text-decoration: none; border-radius: 5px; font-weight: bold; transition: background-color 0.3s; }
        .contact-btn:hover { background-color: #2980b9; }
    </style>
</head>
<body>

<header>
    <h1>Arpit Shringi</h1>
    <p>Web Developer & Data Analyst</p>
</header>

<div class="container">
    <h2 class="section-title">My Expertise</h2>
    
    <div class="skills-grid">
        <!-- Web Development -->
        <div class="skill-card">
            <div class="skill-icon">🌐</div>
            <div class="skill-title">Web Development</div>
            <div class="skill-desc">Building responsive, user-friendly, and professional websites tailored to business needs, ensuring seamless functionality and modern design.</div>
        </div>

        <!-- Data Analysis -->
        <div class="skill-card">
            <div class="skill-icon">📊</div>
            <div class="skill-title">Data Analysis & Strategy</div>
            <div class="skill-desc">Specializing in end-to-end site analysis and data-driven A/B testing strategies. I build dynamic dashboards to track performance across various business scenarios, delivering comprehensive executive summaries and clear action plans.</div>
        </div>

        <!-- Data Cleaning -->
        <div class="skill-card">
            <div class="skill-icon">🧹</div>
            <div class="skill-title">Data Cleaning & Management</div>
            <div class="skill-desc">Ensuring absolute data integrity by meticulously preprocessing and structuring complex datasets. I excel at organizing raw data using tools like Google Sheets, making it fully ready for scenario testing, seamless dashboard integration, and accurate analysis.</div>
        </div>
    </div>
</div>

<footer>
    <p>Ready to collaborate? Let's build something great together.</p>
    <a href="mailto:arpitshringi05@gmail.com" class="contact-btn">✉️ Email Me</a>
    <p style="margin-top: 20px; font-size: 0.9rem;">&copy; 2026 Arpit Shringi</p>
</footer>

</body>
</html>
