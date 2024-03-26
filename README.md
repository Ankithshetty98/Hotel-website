
async function createUserSession(req, res) {
  const { email } = req.userDetails;

  try {
    const user = await userService.getUserByEmail(email);

    if (!user) {
      return res.redirect(`${process.env.DOMAIN_URL}/validate-sso-token?error=User Not registered with BrightLab`);
    }

    const ssoResponse = loginDto.createSSORespSameAsDeep(user);
    const session = await sessionService.createSession(user, ssoResponse);
    const token = cassandraUtil.generateTimeuuid();

    await redisUtil.save(token, session, 60); // Expires after 60 seconds

    res.cookie('Authorization', session, { httpOnly: true });
    return res.redirect(`${process.env.DOMAIN_URL}/validate-sso-token?token=${token}`);
  } catch (error) {
    console.error(error);
    // Handle error appropriately, e.g., redirect to error page
  }
}
# Used libraries

- JavaScript
- CSS
- Bootstrap5
- Bootstrap-icons
- Swiper
- Isotope
- Animate.css


